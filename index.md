# Sport-FM - Tu tienda deportiva favorita

![Banner Sport-FM](https://via.placeholder.com/1200x300) <!-- Reemplazar con imagen real -->

## Productos Destacados

<div class="image-grid">
<?php while($producto = $destacados->fetch_assoc()): ?>
    <div class="carousel-item" style="position:relative;" id="producto-<?= $producto['id'] ?>">
        <?php
            $query_cantidad = "SELECT cantidad FROM productos WHERE id = " . intval($producto['id']);
            $res_cantidad = $conn->query($query_cantidad);
            $cantidad = $res_cantidad ? $res_cantidad->fetch_assoc()['cantidad'] : 0;

            $query_calif = "SELECT calificacion FROM productos_destacados WHERE producto_id = " . intval($producto['id']);
            $res_calif = $conn->query($query_calif);
            $calificacion = $res_calif && $res_calif->num_rows > 0 ? $res_calif->fetch_assoc()['calificacion'] : null;
        ?>
        
        <?php if($calificacion): ?>
        <div class="calificacion-letrero">
            ⭐ <?= number_format($calificacion, 1) ?>
        </div>
        <?php endif; ?>
        
        <div id="cantidad-<?= $producto['id'] ?>" class="cantidad-disponible">
            <?= $cantidad ?> disponibles
        </div>
        
        ![<?= htmlspecialchars($producto['nombre']) ?>](<?= htmlspecialchars($producto['imagen']) ?>)
        
        ### <?= htmlspecialchars($producto['nombre']) ?>
        
        **Precio:** $<?= number_format($producto['precio'], 2) ?>
        
        <div class="acciones-producto">
            <input type="number" min="1" max="<?= $cantidad ?>" value="1" id="input-<?= $producto['id'] ?>">
            <button class="btn-comprar">Comprar</button>
            <button class="btn-comprar btn-carrito" data-id="<?= $producto['id'] ?>" data-max="<?= $cantidad ?>">Añadir al carrito</button>
        </div>
    </div>
<?php endwhile; ?>
</div>

## Productos en Rebaja <span style="color:#e53935;">🔥</span>

<div class="carousel-container">
<?php if ($ofertas && $ofertas->num_rows > 0): ?>
    <?php while($oferta = $ofertas->fetch_assoc()): ?>
    <div class="carousel-item" style="position:relative;">
        ![<?= htmlspecialchars($oferta['nombre']) ?>](<?= htmlspecialchars($oferta['imagen']) ?>)
        
        <div class="precios-rebaja">
            **Oferta:** <span class="sale-price">$<?= number_format($oferta['precio_oferta'], 2) ?></span>
            <span class="old-price">$<?= number_format($oferta['precio'], 2) ?></span>
        </div>
        
        ### <?= htmlspecialchars($oferta['nombre']) ?>
        
        <?php
            $query_cantidad = "SELECT cantidad FROM productos WHERE id = " . intval($oferta['id']);
            $res_cantidad = $conn->query($query_cantidad);
            $cantidad = $res_cantidad ? $res_cantidad->fetch_assoc()['cantidad'] : 0;
        ?>
        <div class="cantidad-disponible"><?= $cantidad ?> disponibles</div>
    </div>
    <?php endwhile; ?>
<?php else: ?>
    <div class="no-offers">
        *Actualmente no hay ofertas disponibles*
    </div>
<?php endif; ?>
</div>

<style>
/* Conserva los mismos estilos CSS */
.image-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
}

.carousel-item {
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: 5px;
    text-align: center;
}

.calificacion-letrero {
    position: absolute;
    top: 10px;
    right: 10px;
    background: white;
    padding: 5px;
    border-radius: 3px;
    font-weight: bold;
}

.sale-price {
    color: #e53935;
    font-weight: bold;
}

.old-price {
    text-decoration: line-through;
    color: #777;
    margin-left: 5px;
}
</style>

<script>
// Conserva los mismos scripts
document.addEventListener('DOMContentLoaded', function() {
    // Event listeners para botones de carrito
    document.querySelectorAll('.btn-carrito').forEach(button => {
        button.addEventListener('click', function() {
            const productId = this.getAttribute('data-id');
            const quantity = document.getElementById(`input-${productId}`).value;
            const maxQuantity = this.getAttribute('data-max');
            
            if(parseInt(quantity) > parseInt(maxQuantity)) {
                alert('No hay suficiente stock');
                return;
            }
            
            // Lógica para añadir al carrito
            console.log(`Añadir al carrito: ID ${productId}, Cantidad ${quantity}`);
        });
    });
});

// Variable global para JS con el usuario actual
window.usuarioActual = "<?= htmlspecialchars($usuario) ?>";
</script>
