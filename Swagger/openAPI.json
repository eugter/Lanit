openapi: 3.0.3
info:
  title: User Service и Сервис авторизации API - СЭМД 
  description: |
    Микросервисы для управления пользователями и аутентификацией
    в Автоматизированной информационной системе электронных медицинских документов
  version: 1.0.0
  contact:
    name: Development Team
    email: dev@polyclinic.ru

servers:
  - url: https://api.polyclinic.ru/user-service/v1
    description: Production server
  - url: https://api-stage.polyclinic.ru/user-service/v1
    description: Staging server

paths:
  /auth/login:
    post:
      summary: Аутентификация пользователя
      description: Выполняет вход пользователя в систему и возвращает токен доступа
      tags:
        - Authentication
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Успешная аутентификация
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LoginResponse'
        '401':
          description: Неверные учетные данные
        '423':
          description: Учетная запись заблокирована

  /users:
    post:
      summary: Создание нового пользователя
      description: Создает учетную запись для сотрудника поликлиники
      tags:
        - Users
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: Пользователь успешно создан
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserResponse'
        '400':
          description: Неверные данные пользователя
        '409':
          description: Пользователь с таким email/логином уже существует

    get:
      summary: Поиск и фильтрация пользователей
      description: Возвращает список пользователей с поддержкой пагинации и фильтрации
      tags:
        - Users
      security:
        - bearerAuth: []
      parameters:
        - name: role
          in: query
          schema:
            type: string
            enum: [DOCTOR, REGISTRAR, MANAGER, ANALYST]
          description: Фильтр по роли
        - name: department
          in: query
          schema:
            type: string
          description: Фильтр по отделению
        - name: page
          in: query
          schema:
            type: integer
            minimum: 0
            default: 0
          description: Номер страницы
        - name: size
          in: query
          schema:
            type: integer
            minimum: 1
            maximum: 100
            default: 20
          description: Размер страницы
      responses:
        '200':
          description: Успешный запрос
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserListResponse'

  /users/{userId}:
    get:
      summary: Получение информации о пользователе
      description: Возвращает детальную информацию о пользователе по ID
      tags:
        - Users
      security:
        - bearerAuth: []
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Успешный запрос
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserResponse'
        '404':
          description: Пользователь не найден

    put:
      summary: Обновление данных пользователя
      description: Обновляет информацию о пользователе
      tags:
        - Users
      security:
        - bearerAuth: []
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
            format: uuid
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateUserRequest'
      responses:
        '200':
          description: Данные пользователя обновлены
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserResponse'
        '404':
          description: Пользователь не найден

    delete:
      summary: Деактивация пользователя
      description: Деактивирует учетную запись пользователя (мягкое удаление)
      tags:
        - Users
      security:
        - bearerAuth: []
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
            format: uuid
      responses:
        '204':
          description: Пользователь деактивирован
        '404':
          description: Пользователь не найден

components:
  schemas:
    LoginRequest:
      type: object
      required:
        - login
        - password
      properties:
        login:
          type: string
          description: Логин или email пользователя
          example: "ivanov.doctor"
        password:
          type: string
          format: password
          example: "securePassword123"
        twoFactorCode:
          type: string
          description: Код двухфакторной аутентификации
          example: "123456"

    LoginResponse:
      type: object
      properties:
        accessToken:
          type: string
          description: JWT токен для доступа к API
          example: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
        refreshToken:
          type: string
          description: Токен для обновления access token
        tokenType:
          type: string
          default: "Bearer"
        expiresIn:
          type: integer
          description: Время жизни токена в секундах
          example: 3600
        user:
          $ref: '#/components/schemas/UserResponse'

    CreateUserRequest:
      type: object
      required:
        - email
        - login
        - firstName
        - lastName
        - role
      properties:
        email:
          type: string
          format: email
          example: "ivanov@polyclinic.ru"
        login:
          type: string
          example: "ivanov.doctor"
        password:
          type: string
          format: password
          minLength: 8
          example: "defaultPassword123"
        firstName:
          type: string
          example: "Иван"
        lastName:
          type: string
          example: "Иванов"
        middleName:
          type: string
          example: "Петрович"
        role:
          type: string
          enum: [DOCTOR, REGISTRAR, MANAGER, ANALYST]
          example: "DOCTOR"
        department:
          type: string
          example: "Терапевтическое отделение №1"
        specialization:
          type: string
          example: "Терапевт"
        phoneNumber:
          type: string
          example: "+79991234567"

    UpdateUserRequest:
      type: object
      properties:
        email:
          type: string
          format: email
        firstName:
          type: string
        lastName:
          type: string
        middleName:
          type: string
        role:
          type: string
          enum: [DOCTOR, REGISTRAR, MANAGER, ANALYST]
        department:
          type: string
        specialization:
          type: string
        phoneNumber:
          type: string
        isActive:
          type: boolean

    UserResponse:
      type: object
      properties:
        id:
          type: string
          format: uuid
          example: "550e8400-e29b-41d4-a716-446655440000"
        email:
          type: string
          format: email
        login:
          type: string
        firstName:
          type: string
        lastName:
          type: string
        middleName:
          type: string
        role:
          type: string
          enum: [DOCTOR, REGISTRAR, MANAGER, ANALYST]
        department:
          type: string
        specialization:
          type: string
        phoneNumber:
          type: string
        isActive:
          type: boolean
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time

    UserListResponse:
      type: object
      properties:
        content:
          type: array
          items:
            $ref: '#/components/schemas/UserResponse'
        totalElements:
          type: integer
          example: 1500
        totalPages:
          type: integer
          example: 75
        pageNumber:
          type: integer
          example: 0
        pageSize:
          type: integer
          example: 20

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: JWT токен, полученный при аутентификации