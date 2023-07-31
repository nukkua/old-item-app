<script>
  import { v4 } from "uuid";
  import Noty from "noty";

  import "noty/lib/noty.css";
  import "noty/lib/themes/sunset.css";

  let products = [
    {
      id: "1",
      name: "Hp Pavilion Notebook",
      description: "HP Laptop",
      category: "laptop",
    },
    {
      id: "2",
      name: "Mouse Razer",
      description: "Gaming Mouse",
      category: "peripherals",
    },
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: "",
  };
  let editStatus = false;

  const cleanProduct = () => {
    product = { id: "", name: "", description: "", category: "", imageURL: "" };
  };

  const addProduct = () => {
    const newProduct = {
      id: v4(),
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL,
    };

    products = products.concat(newProduct);
    console.log(products);
    cleanProduct();
  };
  const updateProduct = () => {
    let updatedProduct = {
      name: product.name,
      description: product.description,
      id: product.id,
      imageURL: product.imageURL,
      category: product.category,
    };
    const productIndex = products.findIndex((p) => p.id === product.id);
    if (productIndex !== -1) {
      products[productIndex] = updatedProduct;
    }

    cleanProduct();
    editStatus = false;
    new Noty({
      theme: "sunset",
      type: "success",
      timeout: 3000,
      text: "Product Updated Successfully",
    }).show();
  };

  const onSubmitHandler = () => {
    if (!editStatus) {
      addProduct();
    } else {
      updateProduct();
    }
  };

  const deleteProduct = (id) => {
    products = products.filter((product) => product.id !== id);
  };
  const editProduct = (productEdited) => {
    product = productEdited;
    editStatus = true;
  };
</script>

<main>
  <div class="container p-4">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageURL}
                  <img src="static/NotFound.png" alt="" class="img-fluid p-2" />
                {:else}
                  <img src={product.imageURL} alt="" class="img-fluid p-2" />
                {/if}
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{product.name}</strong>
                    <span>
                      <small>
                        {product.category}
                      </small>
                    </span>
                  </h5>

                  <p class="card-text">{product.description}</p>
                  <button
                    class="btn btn-danger"
                    on:click={()=>deleteProduct(product.id)}
                  >
                    Delete
                  </button>
                  <button
                    class="btn btn-secondary"
                    on:click={()=>editProduct(product)}>Edit</button
                  >
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>

      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={onSubmitHandler}>
              <div class="form-group">
                <input
                  bind:value={product.name}
                  type="text"
                  placeholder="Product Name"
                  class="form-control"
                  id="product-name"
                />
              </div>
              <div class="form-group">
                <textarea
                  bind:value={product.description}
                  id="product-description"
                  rows="3"
                  class="form-control"
                  placeholder="Product Description"
                />
              </div>
              <div class="form-group">
                <input
                  bind:value={product.imageURL}
                  type="url"
                  class="form-control"
                  id="product-image-url"
                  placeholder="https://leverna.com"
                />
              </div>
              <div class="form-group">
                <select
                  id="category"
                  class="form-control"
                  bind:value={product.category}
                >
                  <option value="laptop">Laptop</option>
                  <option value="peripherals">Peripherals</option>
                  <option value="servers">Servers</option>
                </select>
              </div>
              <button class="btn btn-secondary">
                {#if !editStatus}
                  Save
                {:else}
                  Update
                {/if}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>

<style>
</style>
