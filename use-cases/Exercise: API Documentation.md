 🧾 Original API Endpoint Code
```js
// Get all products with filtering and pagination
productRouter.get('/', async (req, res) => {
  try {
    const {
      category,
      minPrice,
      maxPrice,
      sort = 'createdAt',
      order = 'desc',
      page = 1,
      limit = 20,
      inStock
    } = req.query;

    const filter = {};
    if (category) filter.category = category;
    if (minPrice !== undefined || maxPrice !== undefined) {
      filter.price = {};
      if (minPrice !== undefined) filter.price.$gte = parseFloat(minPrice);
      if (maxPrice !== undefined) filter.price.$lte = parseFloat(maxPrice);
    }
    if (inStock === 'true') filter.stockQuantity = { $gt: 0 };

    const skip = (parseInt(page) - 1) * parseInt(limit);
    const sortOptions = {};
    sortOptions[sort] = order === 'asc' ? 1 : -1;

    const products = await ProductModel.find(filter)
      .sort(sortOptions)
      .skip(skip)
      .limit(parseInt(limit));

    const totalProducts = await ProductModel.countDocuments(filter);

    return res.status(200).json({
      products,
      pagination: {
        total: totalProducts,
        page: parseInt(page),
        limit: parseInt(limit),
        pages: Math.ceil(totalProducts / parseInt(limit))
      }
    });
  } catch (error) {
    console.error('Error fetching products:', error);
    return res.status(500).json({
      error: 'Server error',
      message: 'Failed to fetch products'
    });
  }
});

// Get product by ID
productRouter.get('/:productId', async (req, res) => {
  try {
    const { productId } = req.params;
    const product = await ProductModel.findById(productId);

    if (!product) {
      return res.status(404).json({
        error: 'Not found',
        message: 'Product not found'
      });
    }

    return res.status(200).json(product);
  } catch (error) {
    console.error('Error fetching product:', error);
    if (error.name === 'CastError') {
      return res.status(400).json({
        error: 'Invalid ID',
        message: 'Invalid product ID format'
      });
    }
    return res.status(500).json({
      error: 'Server error',
      message: 'Failed to fetch product'
    });
  }
});
```

---

## 📘 Prompt 1: Comprehensive Endpoint Documentation

### **GET /products**
- **Description:** Retrieve all products with optional filtering, sorting, and pagination.
- **Query Parameters:**
  - `category` (string): Filter by product category.
  - `minPrice` (number): Minimum price filter.
  - `maxPrice` (number): Maximum price filter.
  - `sort` (string): Field to sort by (default: `createdAt`).
  - `order` (string): Sort order (`asc` or `desc`, default: `desc`).
  - `page` (integer): Page number (default: 1).
  - `limit` (integer): Number of items per page (default: 20).
  - `inStock` (boolean as string): If `"true"`, only return products with stock > 0.
- **Response:**
  - `200 OK`: JSON object with `products` array and `pagination` metadata.
  - `500 Server Error`: `{ error: "Server error", message: "Failed to fetch products" }`
- **Authentication:** None required.
- **Example Request:**
  ```
  GET /products?category=electronics&minPrice=100&maxPrice=500&page=2&limit=10
  ```
- **Example Response:**
  ```json
  {
    "products": [
      { "id": "123", "name": "Smartphone", "price": 299 }
    ],
    "pagination": {
      "total": 45,
      "page": 2,
      "limit": 10,
      "pages": 5
    }
  }
  ```

---

### **GET /products/:productId**
- **Description:** Retrieve a single product by its ID.
- **Path Parameters:**
  - `productId` (string): Unique identifier of the product.
- **Response:**
  - `200 OK`: Product object.
  - `404 Not Found`: `{ error: "Not found", message: "Product not found" }`
  - `400 Bad Request`: `{ error: "Invalid ID", message: "Invalid product ID format" }`
  - `500 Server Error`: `{ error: "Server error", message: "Failed to fetch product" }`
- **Authentication:** None required.
- **Example Request:**
  ```
  GET /products/123
  ```
- **Example Response:**
  ```json
  {
    "id": "123",
    "name": "Smartphone",
    "price": 299,
    "category": "electronics"
  }
  ```

---

## 📖 Prompt 2: API Reference Conversion (OpenAPI 3.0)

```yaml
openapi: 3.0.0
info:
  title: Product API
  version: 1.0.0
paths:
  /products:
    get:
      summary: Get all products with filtering and pagination
      parameters:
        - in: query
          name: category
          schema: { type: string }
        - in: query
          name: minPrice
          schema: { type: number }
        - in: query
          name: maxPrice
          schema: { type: number }
        - in: query
          name: sort
          schema: { type: string, default: createdAt }
        - in: query
          name: order
          schema: { type: string, enum: [asc, desc], default: desc }
        - in: query
          name: page
          schema: { type: integer, default: 1 }
        - in: query
          name: limit
          schema: { type: integer, default: 20 }
        - in: query
          name: inStock
          schema: { type: string, enum: [true] }
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  products:
                    type: array
                    items: { type: object }
                  pagination:
                    type: object
        '500':
          description: Server error
  /products/{productId}:
    get:
      summary: Get product by ID
      parameters:
        - in: path
          name: productId
          required: true
          schema: { type: string }
      responses:
        '200':
          description: Product found
        '404':
          description: Product not found
        '400':
          description: Invalid product ID format
        '500':
          description: Server error
```

---

 Prompt 3: Developer Usage Guide

**Audience:** Junior developers familiar with REST APIs but new to this project.  
**Tone:** Friendly and technical.

Using the Product API

1. **Authentication:**  
   No authentication is required for these endpoints.

2. **Formatting Requests:**  
   - Use query parameters to filter, sort, and paginate results.  
   - Example:  
     ```http
     GET /products?category=books&minPrice=10&maxPrice=50&page=1&limit=5
     ```

3. **Handling Responses:**  
   - Successful responses return JSON with product data and pagination info.  
   - Example:  
     ```json
     {
       "products": [{ "id": "1", "name": "Book A", "price": 15 }],
       "pagination": { "total": 12, "page": 1, "limit": 5, "pages": 3 }
     }
     ```

4. **Common Errors:**  
   - `400 Invalid ID`: Ensure product IDs are valid MongoDB ObjectIds.  
   - `404 Not Found`: Product doesn’t exist.  
   - `500 Server Error`: Internal issue; retry later.

5. **Example Code (JavaScript with Fetch):**
   ```js
   async function getProducts() {
     const response = await fetch('/products?category=electronics&limit=10');
     if (!response.ok) throw new Error('Error fetching products');
     const data = await response.json();
     console.log(data.products);
   }

   async function getProductById(id) {
     const response = await fetch(`/products/${id}`);
     if (response.status === 404) {
       console.error('Product not found');
       return;
     }
     const product = await response.json();
     console.log(product);
   }
   ```

---

 Reflection
- **Most challenging to document:** Error handling and pagination details (needed careful review of code).  
- **Prompt adjustments:** Adding explicit examples improved clarity.  
- **Most effective format:** OpenAPI for machine-readable specs, Markdown for developer guides.  
- **Workflow use:** I’d generate OpenAPI first, then layer on a friendly usage guide for developers.
