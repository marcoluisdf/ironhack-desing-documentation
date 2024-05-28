# ironhack-desing-documentation
ironhack-desing-documentation


# Lab Overview and Setup:
Introduction to the Scenario:

Participants are tasked with designing an API for an e-commerce system. The system’s requirements include managing user accounts, processing orders, and handling customer interactions efficiently.

Tools and Resources:

Participants will use SwaggerHub, an integrated API design and documentation platform, to create their API specifications. They will be provided with access to SwaggerHub and any necessary guides on its usage.

API Design Session:

Endpoint Design:

Participants will define RESTful API endpoints for user management, order processing, and customer service interactions.

Resource Modeling:

Define the data structures and resource models required to support the API functionality, such as user profiles, order details, and customer tickets.

Documentation:

Document each API endpoint, specifying required parameters, possible responses, and error codes to ensure clarity and completeness. Emphasis will be placed on adherence to best practices in API documentation to enhance ease of use and maintainability.

``` yaml
openapi: 3.0.0
info:
  title: E-commerce API
  version: 1.0.0
  description: |
    This API provides functionalities for an e-commerce system, allowing for management of users, shopping carts, service tickets, and payments. It supports CRUD operations for users, adding and removing items from shopping carts, managing service tickets, and processing payments.
servers:
  - url: http://localhost:8080/v1
paths:
  /users:
    post:
      summary: Create a new user
      description: Register a new user in the system. Users can have different profiles such as customers, system users, or administrators.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                id:
                  type: integer
                  example: 1
                name:
                  type: string
                  example: "John Doe"
                email:
                  type: string
                  example: "john.doe@example.com"
                status:
                  type: string
                  example: "active"
                active:
                  type: boolean
                  example: true
                profile:
                  type: string
                  enum: [customer, systemUser, admin]
                  example: "customer"
      responses:
        '201':
          description: User created successfully
        '400':
          description: Bad request - Invalid input
        '500':
          description: Internal server error

  /users/{id}:
    get:
      summary: Get user by ID
      description: Retrieve details of a specific user by their unique ID.
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: integer
            example: 1
      responses:
        '200':
          description: User found successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: User not found
        '500':
          description: Internal server error
    put:
      summary: Update user data
      description: Update the details of an existing user by their unique ID.
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: integer
            example: 1
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                name:
                  type: string
                  example: "Jane Doe"
                email:
                  type: string
                  example: "jane.doe@example.com"
                status:
                  type: string
                  example: "inactive"
                active:
                  type: boolean
                  example: false
                profile:
                  type: string
                  enum: [customer, systemUser, admin]
                  example: "systemUser"
      responses:
        '200':
          description: User updated successfully
        '400':
          description: Bad request - Invalid input
        '404':
          description: User not found
        '500':
          description: Internal server error

  /cart:
    post:
      summary: Add item to cart
      description: Add an item to a user's shopping cart.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                cartId:
                  type: integer
                  example: 101
                userId:
                  type: integer
                  example: 1
                productId:
                  type: integer
                  example: 202
                quantity:
                  type: integer
                  example: 3
                unitPrice:
                  type: number
                  format: float
                  example: 29.99
      responses:
        '201':
          description: Item added to cart successfully
        '400':
          description: Bad request - Invalid input
        '500':
          description: Internal server error

  /cart/{cartId}:
    get:
      summary: Get cart by ID
      description: Retrieve the contents of a specific shopping cart by its unique ID.
      parameters:
        - in: path
          name: cartId
          required: true
          schema:
            type: integer
            example: 101
      responses:
        '200':
          description: Cart found successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Cart'
        '404':
          description: Cart not found
        '500':
          description: Internal server error
    delete:
      summary: Remove item from cart
      description: Remove an item from a specific shopping cart by its unique ID.
      parameters:
        - in: path
          name: cartId
          required: true
          schema:
            type: integer
            example: 101
      responses:
        '200':
          description: Item removed from cart successfully
        '404':
          description: Cart not found
        '500':
          description: Internal server error

  /tickets:
    post:
      summary: Create a service ticket
      description: Create a new service ticket related to a user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                status:
                  type: string
                  example: "open"
                ticketId:
                  type: integer
                  example: 301
                userId:
                  type: integer
                  example: 1
                comments:
                  type: string
                  example: "User reported an issue with the product."
      responses:
        '201':
          description: Ticket created successfully
        '400':
          description: Bad request - Invalid input
        '500':
          description: Internal server error

  /tickets/{id}:
    get:
      summary: Get ticket by ID
      description: Retrieve the details of a specific service ticket by its unique ID.
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: integer
            example: 301
      responses:
        '200':
          description: Ticket found successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Ticket'
        '404':
          description: Ticket not found
        '500':
          description: Internal server error
    put:
      summary: Update ticket status
      description: Update the status or comments of an existing service ticket by its unique ID.
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: integer
            example: 301
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                status:
                  type: string
                  example: "closed"
                comments:
                  type: string
                  example: "Issue resolved."
      responses:
        '200':
          description: Ticket status updated successfully
        '400':
          description: Bad request - Invalid input
        '404':
          description: Ticket not found
        '500':
          description: Internal server error

  /payments:
    post:
      summary: Make a purchase
      description: Execute a purchase by providing the cart ID and payment method.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                cartId:
                  type: integer
                  example: 101
                paymentMethod:
                  type: string
                  enum: [Cash, DebitCard, CreditCard]
                  example: "CreditCard"
      responses:
        '201':
          description: Purchase made successfully
        '400':
          description: Bad request - Invalid input
        '404':
          description: Cart not found
        '500':
          description: Internal server error

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
          example: 1
        name:
          type: string
          example: "John Doe"
        email:
          type: string
          example: "john.doe@example.com"
        status:
          type: string
          example: "active"
        active:
          type: boolean
          example: true
        profile:
          type: string
          enum: [customer, systemUser, admin]
          example: "customer"
    Cart:
      type: object
      properties:
        cartId:
          type: integer
          example: 101
        userId:
          type: integer
          example: 1
        productId:
          type: integer
          example: 202
        quantity:
          type: integer
          example: 3
        unitPrice:
          type: number
          format: float
          example: 29.99
    Ticket:
      type: object
      properties:
        status:
          type: string
          example: "open"
        ticketId:
          type: integer
          example: 301
        userId:
          type: integer
          example: 1
        comments:
          type: string
          example: "User reported an issue with the product."
    Payment:
      type: object
      properties:
        cartId:
          type: integer
          example: 101
        paymentMethod:
          type: string
          enum: [Cash, DebitCard, CreditCard]
          example: "CreditCard"

```

# Desafios
- Durante el diseño de la especificacion los principales desafios fueron terner una amplia perspectiva del desarrollo, ya que estamos acostumbrados a ir incluyendo atributos funcionalidades conforme avanzamos en el desarroollo, y al utlizar Api First se vuelve un poco complejo al inicio, pero nos deja un mayor analisis de lo que desarrollaremos

- Un desafio futuro para la implementacion de Api Firts, es poder incluir y trabajar de la mana con nuestra contraparte(Backend-FrontEnd), ya que con esto el desarrollo se volvera mas agil y no tendremos que esperar a que una parte termine.