<script>
  import { v4 as uuid4 } from 'uuid';

  let isEditable = false;

  let products = [
    {
      id: uuid4(),
      name: 'HP Pavilion Notebook',
      description: 'HP Laptop',
      category: 'Laptop',
      url:
        'https://ssl-product-images.www8-hp.com/digmedialib/prodimg/lowres/c06442921.png',
    },
  ];

  const clearProduct = {
    id: '',
    name: '',
    description: '',
    url: '',
    category: '',
  };

  let product = {
    id: '',
    name: '',
    description: '',
    url: '',
    category: '',
  };

  const handleSubmit = () => {
    if (isEditable) {
      const index = findIndexProduct(product);
      products[index] = product;
    } else {
      product.id = uuid4();
      products = [...products, product];
    }

    product = Object.assign({}, clearProduct);
    isEditable = false;
  };

  const findIndexProduct = (prod) => {
    return products.findIndex((p) => p.id === prod.id);
  };

  const deleteProduct = (prod) => {
    const index = findIndexProduct(prod);
    products = [...products.slice(0, index), ...products.slice(index + 1)];
  };

  const editProduct = (prod) => {
    isEditable = true;
    product = prod;
  };
</script>

<style>

</style>

<main>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                <img
                  src={product.url ? product.url : './images/noproduct.png'}
                  alt="Product_image"
                  class="img-fluid p-2" />
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{product.name}</strong>
                  </h5>
                  <span>
                    <small>{product.category}</small>
                  </span>
                  <p>{product.description}</p>
                  <button
                    class="btn btn-danger"
                    on:click={deleteProduct(product)}>
                    Delete
                  </button>
                  <button
                    class="btn btn-secondary"
                    on:click={editProduct(product)}>
                    Edit
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>

      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit|preventDefault={handleSubmit}>
              <div class="form-group">
                <input
                  class="form-control"
                  bind:value={product.name}
                  type="text"
                  id="name"
                  placeholder="Product name" />
              </div>
              <div class="form-group">
                <textarea
                  class="form-control"
                  bind:value={product.description}
                  id="description"
                  rows="3" />
              </div>
              <div class="form-group">
                <input
                  class="form-control"
                  bind:value={product.url}
                  type="url"
                  id="image_url"
                  placeholder="Url image" />
              </div>
              <div class="form-group">
                <select
                  class="form-control"
                  id="category"
                  bind:value={product.category}>
                  <option value="Laptop">Laptops</option>
                  <option value="Periphericals">Periphericals</option>
                  <option value="Servers">Servers</option>
                </select>
              </div>
              <button class="btn btn-primary">
                {!isEditable ? 'Save product' : 'Edit product'}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>
