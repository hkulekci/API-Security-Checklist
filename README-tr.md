[中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-jp.md)

# API Güvenlik Kontrol Listesi
API tasarlarken, test ederken ya da yayınlarken dikkat edilmesi gereken en önemli güvenlik sorunları için kontrol listesidir.

------------------------------------------------------------------------------
## Authentication
- [ ] `Basic Auth` kullanmayın. Bunun yerine standart authentication kullanın (Örn: JWT, OAuth).
- [ ] `Authentication`, `token generating`, `password storing` gibi kavramlar için tekeri yeniden icat etmeyin. Standart kütüphaneleri kullanın.
- [ ] `Max Retry` ve `jail` özelliklerini giriş sayfalarında kullanın.
- [ ] Tüm önemli verilerinizi şifreleyin.

### JWT (JSON Web Token)
- [ ] Brute force saldırılarını zorlaştırmak için rasgele karmaşık anahtar (`JWT Secret`) kullanın.
- [ ] Algoritmayı payload üzerinden almayın backend üzerinden tutun (`HS256` veya `RS256`)
- [ ] Token için bir sonlanma süresi (`TTL`, `RTTL`) belirleyin ve kısa tutmaya çalışın.
- [ ] Önemli verileri JWT üzerinde tutmayın, [kolayca](https://jwt.io/#debugger-io) çözülebilir.

### OAuth
- [ ] `redirect_uri`nin sunucu tarafında tutulan onaylı bir listede olduğunu her zaman kontrol edin.
- [ ] Her zaman `code` ile değişim yapın `token` ile değil (`response_type=token` şeklinde bir isteği kabul etmeyin).
- [ ] CSRF ataklarını önlemek için OAuth doğrulama işlemlerinde `state` parametresini rasgele bir hash ile kullanın.
- [ ] Varsayılan bir `scope` belirleyin ve her bir uygulama için bunu onaylayın.

## Erişim
- [ ] DDoS/ Brute-Force ataklarını önlemek için peş peşe istek sayısını (Throttling) sınırlandırın.
- [ ] MITM (Man In The Middle Attack)'i önlemek için sunucu tarafında HTTPS kullanın. 
- [ ] SSL Strip atağını önlemek için `HSTS` başlığını SSL ile kullanın.

## Girdiler
- [ ] Use the proper HTTP method according to the operation: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, and `DELETE (to delete a record)`, and respond with `405 Method Not Allowed` if the requested method isn't appropriate for the requested resource.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (e.g. `application/xml`, `application/json`, etc) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (e.g. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc).
- [ ] Validate User input to avoid common vulnerabilities (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, etc).
- [ ] Don't use any sensitive data (`credentials`, `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.
- [ ] Use an API Gateway service to enable caching, Rate Limit policies (e.g. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) and deploy APIs resources dynamically.

## İşleme
- [ ] Check if all the endpoints are protected behind authentication to avoid broken authentication process.
- [ ] User own resource ID should be avoided. Use `/me/orders` instead of `/user/654321/orders`.
- [ ] Don't auto-increment IDs. Use `UUID` instead.
- [ ] If you are parsing XML files, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML files, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Use CDN for file uploads.
- [ ] If you are dealing with huge amount of data, use Workers and Queues to process as much as possible in background and return response fast to avoid HTTP Blocking.
- [ ] Do not forget to turn the DEBUG mode OFF.

## Output
- [ ] Send `X-Content-Type-Options: nosniff` header.
- [ ] Send `X-Frame-Options: deny` header.
- [ ] Send `Content-Security-Policy: default-src 'none'` header.
- [ ] Remove fingerprinting headers - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] Force `content-type` for your response, if you return `application/json` then your response `content-type` is `application/json`.
- [ ] Don't return sensitive data like `credentials`, `Passwords`, `security tokens`.
- [ ] Return the proper status code according to the operation completed. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc).

## CI & CD
- [ ] Birim/Entegrasyon testleri ile tasarımınınızı ve uygulamanızı denetim altına alın.
- [ ] Kodlarınızı kritik sürecinden (code review) geçirin ve kendi kendine onaylama sürecini göz artdı edin.
- [ ] Canlı ortama göndermeden önce, servisinize ait tüm bileşenlerinizin ve hatta `vendor` klasörü, ek kütüphaneleri AV yazılımı tarafından tarandığına emin olun, 
- [ ] Yayınlama süreçlerinde geri alma (rollback) çözümleri tasarlayın.


------------------------------------------------------------------------------

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
