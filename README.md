# Fomoteam Backend - Product CRUD API

This is a RESTful API for managing products built with .NET 10.0 following Clean Architecture principles.

## Architecture

The project follows Clean Architecture with the following layers:

- **Domain Layer**: Contains entities and repository interfaces
  - `Domain/Entities/Product.cs` - Product entity
  - `Domain/Interfaces/IProductRepository.cs` - Repository contract

- **Application Layer**: Contains business logic, services, and DTOs
  - `Application/Services/ProductService.cs` - Business logic for products
  - `Application/DTOs/` - Data Transfer Objects

- **Infrastructure Layer**: Contains data access implementation
  - `Infrastructure/Data/ApplicationDbContext.cs` - EF Core DbContext
  - `Infrastructure/Repositories/ProductRepository.cs` - Repository implementation

- **Presentation Layer**: Contains API controllers
  - `Presentation/Controllers/ProductsController.cs` - API endpoints

## Database Setup

The application uses **MySQL/MariaDB on Laragon** with a database named `fomoteam`.

### Prerequisites
- **Laragon** installed and running
- **MySQL** service started in Laragon

### Connection String
Located in `appsettings.json`:
```json
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost;Port=3306;Database=fomoteam;User=root;Password=;"
}
```

**Note:** If your Laragon MySQL has a password, update the `Password=;` part with your password.

### Migrations
The initial migration has already been created and applied. If you need to recreate the database:

```bash
# Remove existing database
dotnet ef database drop

# Apply migrations
dotnet ef database update
```

## Running the Application

```bash
dotnet run
```

The API will be available at:
- HTTP: `http://localhost:5000`
- HTTPS: `https://localhost:5001`

## API Endpoints

### Get All Products
```
GET /api/products
```
Returns a list of all products.

**Response**: `200 OK`
```json
[
  {
    "id": 1,
    "name": "Product Name",
    "description": "Product Description",
    "price": 99.99,
    "stock": 100,
    "createdAt": "2024-01-01T00:00:00Z",
    "updatedAt": null
  }
]
```

### Get Product by ID
```
GET /api/products/{id}
```
Returns a specific product by ID.

**Response**: `200 OK` or `404 Not Found`

### Create Product
```
POST /api/products
Content-Type: application/json

{
  "name": "New Product",
  "description": "Product Description",
  "price": 99.99,
  "stock": 100
}
```

**Response**: `201 Created`
```json
{
  "id": 1,
  "name": "New Product",
  "description": "Product Description",
  "price": 99.99,
  "stock": 100,
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": null
}
```

**Validation Rules:**
- `name`: Required, max 200 characters
- `description`: Optional, max 1000 characters
- `price`: Required, must be greater than 0
- `stock`: Required, cannot be negative

### Update Product
```
PUT /api/products/{id}
Content-Type: application/json

{
  "name": "Updated Product",
  "description": "Updated Description",
  "price": 149.99,
  "stock": 50
}
```

**Response**: `200 OK` or `404 Not Found`

### Delete Product
```
DELETE /api/products/{id}
```

**Response**: `204 No Content` or `404 Not Found`

## Testing the API

You can test the API using:

1. **Swagger/OpenAPI**: Navigate to the OpenAPI endpoint when running in Development mode
2. **Postman**: Import the endpoints above
3. **curl**: Example commands below

### Example curl Commands

```bash
# Get all products
curl -X GET https://localhost:5001/api/products

# Get product by ID
curl -X GET https://localhost:5001/api/products/1

# Create a product
curl -X POST https://localhost:5001/api/products \
  -H "Content-Type: application/json" \
  -d '{"name":"Test Product","description":"Test Description","price":99.99,"stock":100}'

# Update a product
curl -X PUT https://localhost:5001/api/products/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Updated Product","description":"Updated Description","price":149.99,"stock":50}'

# Delete a product
curl -X DELETE https://localhost:5001/api/products/1
```

## Project Dependencies

- **Pomelo.EntityFrameworkCore.MySql** (9.0.0) - MySQL database provider for EF Core
- **Microsoft.EntityFrameworkCore.Tools** (9.0.0) - EF Core CLI tools
- **Microsoft.EntityFrameworkCore.Design** (9.0.0) - Design-time components

## Development

### Adding New Migrations

When you modify the database schema:

```bash
dotnet ef migrations add MigrationName
dotnet ef database update
```

### Building the Project

```bash
dotnet build
```

## Clean Architecture Benefits

1. **Separation of Concerns**: Each layer has a specific responsibility
2. **Testability**: Business logic is isolated and easy to test
3. **Maintainability**: Changes in one layer don't affect others
4. **Flexibility**: Easy to swap implementations (e.g., change from SQL Server to another database)
5. **Scalability**: Easy to add new features following the same patterns

## Next Steps

- Add authentication and authorization
- Implement caching
- Add logging
- Create unit and integration tests
- Add API documentation with Swagger
- Implement pagination for the GET all products endpoint
- Add filtering and sorting capabilities
