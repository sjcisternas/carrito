# Carrito de compras 

##### Autor: Santiago Cisternas

## Sobre el proyecto:

Este carrito fue realizado como ejercicio de práctica de manipulación de DOM con JavaScript. 

El proyecto fue abordado desde el paradigma de programación funcional, con lo cual, el código puede descomponerse en las siguientes funciones simples:

##### Agregar un producto al carrito

```
const agregarProducto = (e) =>{
    const producto = {
        titulo: e.target.dataset.burger,
        precio: parseInt(e.target.dataset.precio),
        id: e.target.dataset.burger,
        cantidad: 1
    }
    const posicion = carrito.findIndex((item) => item.id === producto.id);
    if(posicion === -1){
        carrito.push(producto);
    }else{
        carrito[posicion].cantidad++;
    }
    mostrarCarrito();
}
```

##### Mostrar el carrito en el DOM

```
const mostrarCarrito = () =>{
    contenedor.textContent = '';
    carrito.forEach((item) => {
        const clone = templateProducto.content.cloneNode(true);
        clone.querySelector('#titulo-item').textContent = item.titulo;
        clone.querySelector('#item-cantidad').textContent = item.cantidad;
        clone.querySelector('#precio-item').textContent = item.precio * item.cantidad;
        clone.querySelector('.btn-outline-success').dataset.id = item.id;
        clone.querySelector('.boton-img-1').dataset.id = item.id;
        clone.querySelector('.btn-danger').dataset.id = item.id;
        clone.querySelector('.boton-img-2').dataset.id = item.id;
        fragment.appendChild(clone);
    });
    contenedor.appendChild(fragment);
    mostrarFooter();
}
```

##### Sumar un producto más

```
const btnAgregar = (e) => {
    carrito = carrito.map((item) => { 
        if (e.target.dataset.id === item.id){ 
            item.cantidad++;
        }
        return item;
    })
    mostrarCarrito();
}
```

##### Eliminar productos agregados

```
const btnDisminuir = (e) => {
    carrito = carrito.filter((item) =>{ 
        if (e.target.dataset.id === item.id){ 
            if (item.cantidad > 0){ 
                item.cantidad--;
                if (item.cantidad === 0) return; 
                return item;
            }
        }else{
            return item;
        }
    })
    mostrarCarrito();
}
```

##### Mostrar el precio total de los productos agregados

```
const mostrarFooter = () => {
    footer.textContent = '';
    const total = carrito.reduce((acc, current) => {
        return acc + current.cantidad * current.precio;
    },0); 
    const clone = templateFooter.content.cloneNode(true); 
    clone.querySelector('.precio_footer').textContent = total;
    footer.appendChild(clone); 

}
```