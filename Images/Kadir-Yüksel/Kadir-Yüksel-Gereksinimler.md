
[Profil 1-5]

1. Üye Olma (Register)
    * **API Metodu:** `POST /auth/register`
    * **Açıklama:** Kullanıcıların sisteme yeni bir hesap ile katılmasını sağlar. 


2. Giriş Yapma (Login)
    * **API Metodu:** `POST /auth/login`
    * **Açıklama:** Kullanıcıların email ve şifre ile sistemde kimlik doğrulaması yapmasını sağlar.


3. Profil Görüntüleme
    * **API Metodu:** `GET /users/{usersId}`
    * **Açıklama:** Kullanıcıların sistemde hesaplarının görüntülenmesini sağlar. 


4. Profil Güncelleme
    * **API Metodu:** `PUT /users/{usersId}`
    * **Açıklama:** Kullanıcıların hesaplarında değişiklik yapmasını sağlar.


5. Profil Silme
    * **API Metodu:** `DELETE /users/{usersId}`
    * **Açıklama:** Kullanıcıların hesaplarını silmesini sağlars.



[Diyet Programı 6-10]

6. Diyet Programı Ekleme
    * **API Metodu:** `POST /diets/meals`
    * **Açıklama:** Kullanıcının gün içinde tükettiği yiyecekleri sisteme kaydetmesini sağlar.

    
7. Diyet Programını Güncelleme 
    * **API Metodu:** `PUT /diets/meals/{mealId}`
    * **Açıklama:** Kullanıcı yanlış bir yiyecek girdiğinde veya porsiyonu değiştirmek istediğinde müdahale edebilmesini sağlar.


8. Diyet Programını Silme
    * **API Metodu:** `DELETE /diets/meals/{mealId}`
    * **Açıklama:** Kullanıcı yanlış bir yiyecek girdiğinde silmesini sağlar.


9. Su Tüketimi Ekleme
    * **API Metodu:** `POST /diets/water`
    * **Açıklama:** Kullanıcının gün içinde içtiği su miktarını ml veya litre cinsinden kaydetmesini sağlar.


10. Hazır Diyet Programı Görüntüleme
    * **API Metodu:** `GET /diets/{dietsId}`
    * **Açıklama:** Sistemdeki tüm hazır diyet programlarını kalori,yağ,karbonhidrat,protein değerleriyle birlikte getirir.
  


[Antreman 11-15]

11. Spor Programı (Antrenman) Oluşturma
    * **API Metodu:** `POST /workouts`
    * **Açıklama:** Kullanıcının yeni bir antrenman rutini oluşturmasını sağlar. 


12. Spor Programı (Antrenman) Silme
    * **API Metodu:** `DELETE /workouts/{workoutsId}`
    * **Açıklama:** Kullanıcının spor programını silmesini sağlar. 


13. Spor Programı (Antrenman) Güncelleme
    * **API Metodu:** `PUT /workouts/{workoutsId}`
    * **Açıklama:** Kullanıcının spor programını güncellemesini sağlar.


14. Hazır Egzersiz Görüntüleme
    * **API Metodu:** `GET /workouts/exercises`
    * **Açıklama:** Sistemdeki tüm hazır egzersizlerden seçim yapmanı sağlar.


15. Vücut Metriklerini (Kilo/Boy) Ekleme
    * **API Metodu:** `POST /users/{userId}/metrics`
    * **Açıklama:** Kullanıcının güncel boy kilo gibi değerleri girmesini sağlar.
