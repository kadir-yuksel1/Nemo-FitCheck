# API Tasarımı - OpenAPI Specification Örneği

**OpenAPI Spesifikasyon Dosyası:** [FitCheck.yaml](FitCheck.yaml)

## OpenAPI Specification

```yaml
openapi: 3.0.3
info:
  title: Spor ve Diyet Yönetim API'si
  version: 1.0.0
  description: >
    Bu API, kullanıcıların vücut metriklerini, beslenme alışkanlıklarını, su tüketimlerini
    ve antrenmanlarını takip edebilecekleri; aynı zamanda adminlerin global kütüphaneleri
    ve kullanıcıları yönetebileceği RESTful bir servistir.
  contact:
    name: Kadir YÜKSEL

      ## Özellikler
    - Kullanıcı yönetimi
    - Spor Programı yönetimi
    - Diet Programı yönetimi
    - JWT tabanlı kimlik doğrulama

servers:
  - url: https://api.vercel.com
    description: Üretim sunucusu (Production)
  - url: https://staging-api.vercel.com
    description: Test sunucusu (Staging)
  - url: https://localhost:3000
    description: Yerel geliştirme sunucusu (Development)

tags:
  - name: Auth
    description: Üyelik ve giriş işlemleri
  - name: Profil
    description: Kullanıcı profil yönetimi
  - name: Metrikler
    description: Vücut ölçüleri ve metrik takibi
  - name: Beslenme ve Su
    description: Öğün ve su tüketimi takibi
  - name: Antrenman
    description: Kişisel antrenman yönetimi
  - name: Kütüphane
    description: Global egzersiz ve diyet verileri
  #[Admin Opsiyonel]
  - name: Admin
    description: Sistem ve kullanıcı yönetimi

security:
  - BearerAuth: []

paths:
  # [Kimlik Doğrulama ve Profil 1-5]
  /auth/register:
    post:
      tags:
        - Auth
      summary: Üye Olma (Register)
      operationId: registerUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/RegisterInput"
      responses:
        "201":
          description: Kullanıcı başarıyla oluşturuldu
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/AuthResponse"
        "400":
          description: Bad Request
          content:
            application/json:
              schema:
                $ref: "#/components/responses/BadRequest"
        "409":
          description: Bu e-posta adresi zaten kullanımda
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error"

  /auth/login:
    post:
      tags:
        - Auth
      summary: Giriş Yapma (Login)
      operationId: loginUser
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/LoginInput"
      responses:
        "200":
          description: Başarılı giriş
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/AuthResponse"
        "400":
          $ref: "#/components/responses/BadRequest"
        "401":
          $ref: "#/components/responses/Unauthorized"

  /users/me:
    get:
      tags:
        - Profil
      summary: Profilimi Görüntüleme
      operationId: getMyProfile
      responses:
        "200":
          description: Profil bilgileri başarıyla getirildi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/UserProfile"
        "401":
          $ref: "#/components/responses/Unauthorized"
    put:
      tags:
        - Profil
      summary: Profilimi Güncelleme
      operationId: updateMyProfile
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/UserProfileInput"
      responses:
        "200":
          description: Profil başarıyla güncellendi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/UserProfile"
        "400":
          $ref: "#/components/responses/BadRequest"
        "401":
          $ref: "#/components/responses/Unauthorized"
    delete:
      tags:
        - Profil
      summary: Hesabımı Silme
      operationId: deleteMyAccount
      responses:
        "204":
          description: Hesap başarıyla silindi/pasife alındı
        "401":
          $ref: "#/components/responses/Unauthorized"

  # [Vücut Metrikleri Takibi 6-7]
  /users/me/metrics:
    post:
      tags:
        - Metrikler
      summary: Yeni Metrik Ekleme
      operationId: addMetric
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/MetricInput"
      responses:
        "201":
          description: Metrik başarıyla eklendi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Metric"
        "400":
          $ref: "#/components/responses/BadRequest"
        "401":
          $ref: "#/components/responses/Unauthorized"
    get:
      tags:
        - Metrikler
      summary: Metrik Geçmişini Getirme
      operationId: getMetrics
      responses:
        "200":
          description: Metrikler listelendi
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Metric"
        "401":
          $ref: "#/components/responses/Unauthorized"

  # [Beslenme ve Su Takibi 8-14]
  /users/me/meals:
    post:
      tags:
        - Beslenme ve Su
      summary: Öğün Ekleme
      operationId: addMeal
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/MealInput"
      responses:
        "201":
          description: Öğün eklendi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Meal"
        "400":
          $ref: "#/components/responses/BadRequest"
        "401":
          $ref: "#/components/responses/Unauthorized"
    get:
      tags:
        - Beslenme ve Su
      summary: Günlük Öğünleri Getirme
      operationId: getMeals
      parameters:
        - name: date
          in: query
          required: false
          description: YYYY-MM-DD formatında tarih filtrelemesi
          schema:
            type: string
            format: date
      responses:
        "200":
          description: Öğünler listelendi
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Meal"
        "401":
          $ref: "#/components/responses/Unauthorized"

  /users/me/meals/{mealId}:
    put:
      tags:
        - Beslenme ve Su
      summary: Öğün Güncelleme
      operationId: updateMeal
      parameters:
        - name: mealId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/MealInput"
      responses:
        "200":
          description: Öğün güncellendi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Meal"
        "400":
          $ref: "#/components/responses/BadRequest"
        "401":
          $ref: "#/components/responses/Unauthorized"
        "404":
          $ref: "#/components/responses/NotFound"
    delete:
      tags:
        - Beslenme ve Su
      summary: Öğün Silme
      operationId: deleteMeal
      parameters:
        - name: mealId
          in: path
          required: true
          schema:
            type: string
      responses:
        "204":
          description: Öğün başarıyla silindi
        "401":
          $ref: "#/components/responses/Unauthorized"
        "404":
          $ref: "#/components/responses/NotFound"

  /users/me/water:
    post:
      tags:
        - Beslenme ve Su
      summary: Su Tüketimi Ekleme
      operationId: addWater
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/WaterInput"
      responses:
        "201":
          description: Su eklendi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Water"
        "400":
          $ref: "#/components/responses/BadRequest"
        "401":
          $ref: "#/components/responses/Unauthorized"
    get:
      tags:
        - Beslenme ve Su
      summary: Günlük Su Tüketimini Getirme
      operationId: getWater
      parameters:
        - name: date
          in: query
          required: false
          schema:
            type: string
            format: date
      responses:
        "200":
          description: Su tüketimi getirildi
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/Water"
        "401":
          $ref: "#/components/responses/Unauthorized"

  /users/me/water/{waterId}:
    delete:
      tags:
        - Beslenme ve Su
      summary: Hatalı Su Tüketimini Silme
      operationId: deleteWater
      parameters:
        - name: waterId
          in: path
          required: true
          schema:
            type: string
      responses:
        "204":
          description: Su kaydı silindi
        "401":
          $ref: "#/components/responses/Unauthorized"
        "404":
          $ref: "#/components/responses/NotFound"

    # [Sistem Kütüphanesi - Global Veriler 19-22]
  /exercises:
    get:
      tags:
        - Kütüphane
      summary: Hazır Egzersizleri Listeleme
      operationId: listExercises
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
      responses:
        "200":
          description: Egzersizler listelendi
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: "#/components/schemas/Exercise"
                  total:
                    type: integer
                  page:
                    type: integer
        "401":
          $ref: "#/components/responses/Unauthorized"

  /exercises/{exerciseId}:
    get:
      tags:
        - Kütüphane
      summary: Tekil Egzersiz Detayı
      operationId: getExercise
      parameters:
        - name: exerciseId
          in: path
          required: true
          schema:
            type: string
      responses:
        "200":
          description: Egzersiz detayı getirildi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Exercise"
        "401":
          $ref: "#/components/responses/Unauthorized"
        "404":
          $ref: "#/components/responses/NotFound"

  # [Admin - Kullanıcı Yönetimi 23-32 - Opsiyonel(!)]
  /admin/users:
    get:
      tags:
        - Admin
      summary: Tüm Kullanıcıları Listeleme
      operationId: listAllUsers
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
      responses:
        "200":
          description: Kullanıcılar listelendi
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: "#/components/schemas/UserProfile"
        "401":
          $ref: "#/components/responses/Unauthorized"
        "403":
          $ref: "#/components/responses/Forbidden"

  /admin/users/{userId}/status:
    put:
      tags:
        - Admin
      summary: Kullanıcı Durumunu Güncelleme
      operationId: updateUserStatus
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                status:
                  type: string
                  enum: [ACTIVE, BANNED]
      responses:
        "200":
          description: Durum güncellendi
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/UserProfile"
        "400":
          $ref: "#/components/responses/BadRequest"
        "401":
          $ref: "#/components/responses/Unauthorized"
        "403":
          $ref: "#/components/responses/Forbidden"
        "404":
          $ref: "#/components/responses/NotFound"

components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: 'İstek başlığına "Authorization: Bearer <token>" eklenmeli.'

  schemas:
    RegisterInput:
      type: object
      required: [email, password, name]
      properties:
        email:
          type: string
          format: email
        password:
          type: string
          minLength: 6
          maxLength: 15
        name:
          type: string

    LoginInput:
      type: object
      required: [email, password]
      properties:
        email:
          type: string
          format: email
        password:
          type: string

    AuthResponse:
      type: object
      properties:
        token:
          type: string
        user:
          $ref: "#/components/schemas/UserProfile"

    UserProfile:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        email:
          type: string
        age:
          type: integer
        gender:
          type: string
        status:
          type: string
          enum: [ACTIVE, BANNED]

    UserProfileInput:
      type: object
      properties:
        name:
          type: string
        age:
          type: integer
        gender:
          type: string

    MetricInput:
      type: object
      required: [height, weight]
      properties:
        height:
          type: number
          description: Boy (cm)
        weight:
          type: number
          description: Kilo (kg)
        bodyFatPercentage:
          type: number
          description: Yağ oranı (%)

    Metric:
      allOf:
        - $ref: "#/components/schemas/MetricInput"
        - type: object
          properties:
            id:
              type: string
            date:
              type: string
              format: date-time

    MealInput:
      type: object
      required: [name, calories]
      properties:
        name:
          type: string
        calories:
          type: integer
        portion:
          type: string

    Meal:
      allOf:
        - $ref: "#/components/schemas/MealInput"
        - type: object
          properties:
            id:
              type: string
            date:
              type: string
              format: date-time

    WaterInput:
      type: object
      required: [amount]
      properties:
        amount:
          type: integer
          description: Mililitre (ml) cinsinden

    Water:
      allOf:
        - $ref: "#/components/schemas/WaterInput"
        - type: object
          properties:
            id:
              type: string
            date:
              type: string
              format: date-time

    Exercise:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        targetMuscleGroup:
          type: string
        description:
          type: string

    Error:
      type: object
      required: [message]
      properties:
        message:
          type: string
          example: "Bir hata oluştu."
        code:
          type: string
          example: "ERROR_CODE"

  responses:
    BadRequest:
      description: Geçersiz istek
      content:
        application/json:
          schema:
            $ref: "#/components/schemas/Error"
    Unauthorized:
      description: Kimlik doğrulama başarısız veya token geçersiz
      content:
        application/json:
          schema:
            $ref: "#/components/schemas/Error"
    Forbidden:
      description: Bu işlemi gerçekleştirmek için yetkiniz yok
      content:
        application/json:
          schema:
            $ref: "#/components/schemas/Error"
    NotFound:
      description: İstenen kaynak bulunamadı 
      content:
        application/json:
          schema:
            $ref: "#/components/schemas/Error"
    InternalServerError:
      description: Sunucu tarafında beklenmeyen bir hata oluştu
      content:
        application/json:
          schema:
            $ref: "#/components/schemas/Error"
```
