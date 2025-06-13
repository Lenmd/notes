## 基本结构

```
xxxxx.yyyyy.zzzzz
Header（头部）.Payload（负载）.Signature（签名）
```

Header
```json
{
  "alg": "HS256",  // 使用的签名算法，如 HS256 或 RS256
  "typ": "JWT"
}

```
Payload
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022,
  "exp": 1516242622
}

```
常见的标准字段：
- `iss`（Issuer）：签发者
- `sub`（Subject）：面向用户
- `aud`（Audience）：接收方
- `exp`（Expiration Time）：过期时间
- `nbf`（Not Before）：不能早于某时间使用
- `iat`（Issued At）：签发时间
- `jti`（JWT ID）：唯一 ID

Signature
对前两部分进行签名：
```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)

```
确保 JWT 未被篡改