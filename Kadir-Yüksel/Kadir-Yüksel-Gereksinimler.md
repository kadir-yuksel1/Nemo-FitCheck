### [Kimlik Doğrulama ve Profil 1-5]

1. **Üye Olma (Register)**
   * **API Metodu:** `POST /auth/register`
   * **Açıklama:** Yeni kullanıcı kaydı oluşturur.
2. **Giriş Yapma (Login)**
   * **API Metodu:** `POST /auth/login`
   * **Açıklama:** Başarılı girişte JWT (Token) döndürür. Bundan sonraki işlemlerde bu token kullanılır.
3. **Profilimi Görüntüleme**
   * **API Metodu:** `GET /users/me`
   * **Açıklama:** Token sahibinin kendi profil bilgilerini getirir. Yetkisiz erişimi engeller.
4. **Profilimi Güncelleme**
   * **API Metodu:** `PUT /users/me`
   * **Açıklama:** Kullanıcının temel bilgilerini (ad, yaş, cinsiyet) günceller.
5. **Hesabımı Silme**
   * **API Metodu:** `DELETE /users/me`
   * **Açıklama:** Kullanıcının kendi hesabını silmesini veya pasife almasını sağlar.



### [Vücut Metrikleri Takibi 6-7]

6. **Yeni Metrik Ekleme**
   * **API Metodu:** `POST /users/me/metrics`
   * **Açıklama:** Güncel boy, kilo ve yağ oranını tarih damgasıyla sisteme ekler.
7. **Metrik Geçmişini Getirme**
   * **API Metodu:** `GET /users/me/metrics`
   * **Açıklama:** Kullanıcının geçmiş metriklerini listeler.



### [Beslenme ve Su Takibi 8-14]

8. **Öğün Ekleme**
   * **API Metodu:** `POST /users/me/meals`
   * **Açıklama:** Belirli bir tarihe tüketilen yiyeceği ve kalorisini ekler.
9. **Günlük Öğünleri Getirme**
   * **API Metodu:** `GET /users/me/meals`
   * **Açıklama:**  Kullanıcının belirli bir günde yediği tüm öğünleri ve toplam kaloriyi listeler.
10. **Öğün Güncelleme**
    * **API Metodu:** `PUT /users/me/meals/{mealId}`
    * **Açıklama:** Yanlış girilen bir öğünün porsiyonunu veya içeriğini düzeltir.
11. **Öğün Silme**
    * **API Metodu:** `DELETE /users/me/meals/{mealId}`
    * **Açıklama:** Eklenen  bir öğünü kaldırır.
12. **Su Tüketimi Ekleme**
    * **API Metodu:** `POST /users/me/water`
    * **Açıklama:** Belirli bir tarihte tüketilen su miktarını (ml) kaydeder.
13. **Hatalı Su Tüketimini Silme/Geri Alma**
    * **API Metodu:** `DELETE /users/me/water/{waterId}`
    * **Açıklama:** Kullanıcının yanlışlıkla girdiği su miktarını silmesini sağlar.
14. **Günlük Su Tüketimini Getirme**
    * **API Metodu:** `GET /users/me/water`
    * **Açıklama:** Belirli bir günün toplam su tüketim miktarını ve eklenen kayıtları getirir.




### [Antrenman Yönetimi 15-18]

15. **Antrenman Oluşturma/Atama**
    * **API Metodu:** `POST /users/me/workouts`
    * **Açıklama:** Belirli bir tarih için kişisel antrenman kaydı oluşturur.
16. **Antrenman Güncelleme**
    * **API Metodu:** `PUT /users/me/workouts/{workoutId}`
    * **Açıklama:** Kişisel antrenmanın detaylarını  günceller.
17. **Antrenman Silme**
    * **API Metodu:** `DELETE /users/me/workouts/{workoutId}`
    * **Açıklama:** Kişisel antrenman kaydını siler.
18. **Günlük Antrenmanı Getirme**
    * **API Metodu:** `GET /users/me/workouts`
    * **Açıklama:** Belirli bir güne atanmış antrenmanları listeler. 



### [Sistem Kütüphanesi - Global Veriler 19-22]

19. **Hazır Egzersizleri Listeleme**
    * **API Metodu:** `GET /exercises`
    * **Açıklama:** Sistemdeki standart egzersizleri özet veriyle listeler.
20. **Tekil Egzersiz Detayı**
    * **API Metodu:** `GET /exercises/{exerciseId}`
    * **Açıklama:** Seçilen bir egzersizin tüm detaylarını getirir.
21. **Hazır Diyet Programlarını Listeleme**
    * **API Metodu:** `GET /diets`
    * **Açıklama:** Sistemdeki şablon diyet programlarını özet olarak listeler.
22. **Tekil Diyet Detayı**
    * **API Metodu:** `GET /diets/{dietId}`
    * **Açıklama:** Seçilen diyet programının içindeki günleri ve öğünleri detaylıca getirir.



#[Admin Tarafı Opsiyonel!]
### [Admin - Kullanıcı Yönetimi 23-26]

23. **Tüm Kullanıcıları Listeleme**
    * **API Metodu:** `GET /admin/users`
    * **Açıklama:** Sistemdeki tüm kullanıcıları listeler.
24. **Belirli Bir Kullanıcının Detayını Görme**
    * **API Metodu:** `GET /admin/users/{userId}`
    * **Açıklama:** Sistem yöneticisinin sorunlu bir kullanıcının tüm verilerini incelemesini sağlar.
25. **Kullanıcı Durumunu Güncelleme**
    * **API Metodu:** `PUT /admin/users/{userId}/status`
    * **Açıklama:** Kullanıcının aktiflik durumunu, banını veya rolünü değiştirir. 
26. **Kullanıcıyı Yasaklama / Silme**
    * **API Metodu:** `DELETE /admin/users/{userId}`
    * **Açıklama:** Kullanıcının sisteme erişimini keser 



### [Admin - Kütüphane Yönetimi 27-32]

27. **Yeni Hazır Egzersiz Ekleme**
    * **API Metodu:** `POST /admin/exercises`
    * **Açıklama:** Kütüphaneye yeni bir hazır egzersiz ekler.
28. **Hazır Egzersizi Güncelleme**
    * **API Metodu:** `PUT /admin/exercises/{exerciseId}`
    * **Açıklama:** Hazır egzersizin detaylarını günceller.
29. **Hazır Egzersizi Silme**
    * **API Metodu:** `DELETE /admin/exercises/{exerciseId}`
    * **Açıklama:** Hazır egzersizi pasife alır.
30. **Yeni Diyet Programı Ekleme**
    * **API Metodu:** `POST /admin/diets`
    * **Açıklama:** Yeni şablon diyet paketi ekler.
31. **Diyet Programını Güncelleme**
    * **API Metodu:** `PUT /admin/diets/{dietId}`
    * **Açıklama:** Diyet programının içeriğini günceller.
32. **Diyet Programını Silme**
    * **API Metodu:** `DELETE /admin/diets/{dietId}`
    * **Açıklama:** Diyet programını yayından kaldırır.