<script>

import {v4} from 'uuid';
import Noty from 'noty';

import 'noty/lib/noty.css';
import 'noty/lib/themes/sunset.css'

  //arreglo de productos para llenar las tareas

  let products = [
    {
      id: 1,
      name: "HP Pavilion Notebook",
      description: "HP Laptop",
      category: "laptop"
    },
    {
      id: 2,
      name: "Mause Razer",
      description: "Gaming Mouse",
      category: "Peripherials"
    }
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: ""
  };

  
  //variable para almacenar el estado
  let editStatus = false;


  //colocar registros en blanco
  const cleanProduct = () => {
      product = {
        id: "",
      name: "",
      description: "",
      category: "",
      imageURL: ""
      }
    }

  //funcion add
  const addProduct = () => {
    //console.log(product);
    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL
    }
    products = products.concat(newProduct); //enviar nuevo producto al array
    cleanProduct(); //limpia el formulario
    console.log(products);
  }

  //funcion update
  const updateProduct = () => {
    let updatedProduct = {
      name: product.name,
      description: product.description,
      id: product.id,
      imageURL: product.imageURL,
      category: product.category
    }

    const productIndex = products.findIndex(p => p.id === product.id)
    //console.log(productIndex);
    products[productIndex] = updatedProduct;
    cleanProduct();
    editStatus = false;
    new Noty({
      theme: 'sunset',
      type: 'success',
      timeout: 3000,
      text: 'Produc Updated Succesfully'
    }).show();
  }

  const onSubmitHander = e => {
    //e.preventDefault();

    if (!editStatus){
      addProduct();
    } else {
      updateProduct();
    }
  };

  //funcion delete
  const deleteProduct = (id) => {
    //console.log(id);
    products = products.filter(product => product.id !== id);
  }

  //funcion edit
  const editProduct = productEdited =>{
    product = productEdited;
    editStatus = true;
  }


</script>

<style>

</style>

<main>

  <div class="container p-4">
    <div class="row">
      <div class="col-md-6" >
	  	{#each products as product}
		  <div class="card mt-2">
		  		<div class="row">
					<div class="col-md-4">
						{#if !product.imageURL}
              <img src="images/no-product.png" alt="" class="img-fluid p-2">
            {:else}
              <img src="{product.imageURL}" alt="" class="img-fluid p-2">
            {/if}
					</div>

					<div class="col-md-8">
						<div class="card-body">
              <h5>
							  <strong>{product.name}</strong>
							  <span>
								  <small>{product.category}</small>
							  </span>
						  </h5>

						  <p card-text>{product.description}</p>
              <button class="btn btn-danger" on:click={deleteProduct(product.id)}>Delete</button>
              <button class="btn btn-secondary" on:click={editProduct(product)}>Edit</button>
            </div>
					</div>
				</div>
		  </div>
		{/each}
	  </div>
	  <div class="col-md-6">
        	<div class="card mt-2">
          		<div class="card-body">
            	<form on:submit|preventDefault  ={onSubmitHander}>

              		<div class="form-group">
                	<input
                  bind:value={product.name}
                  type="text"
                  placeholder="Product Name"
                  id="product-name"
                  class="form-control" />
              </div>

              <div class="form-group">
                <textarea
                  bind:value={product.description}
                  id="product-description"
                  rows="3"
                  placeholder="Product Description"
                  class="form-control" />
              </div>

              <div class="form-group">
                <input
                  bind:value={product.imageURL}
                  type="url"
                  id="product-image-url"
                  placeholder="http://abc..."
                  class="form-control" />
              </div>

              <div class="form-group">
                <select
                  bind:value={product.category}
                  id="category"
                  class="form-control">
                  <option value="laptops">Laptops</option>
                  <option value="peripherials">Peripherials</option>
                  <option value="servers">Servers</option>
                </select>
              </div>

              <button class="btn btn-secondary">
              {#if !editStatus}Save Product{:else}Update Product{/if}
              </button>
            </form>
          </div>
        </div>
      </div>
      	
    </div>
  

</main>
