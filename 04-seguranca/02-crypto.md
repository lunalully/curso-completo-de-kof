# Módulo 04 · Aula 2 — Criptografia

> Digests, HMAC, AES-GCM e aleatório seguro. Resultados idênticos entre
> JVM/Native/JS para as funções cross-target.

## Digests e HMAC

```kof
main() {
    println(crypto.sha256("kof"))               // hex (64 chars)
    println(crypto.sha512("kof"))               // hex (128 chars) — JVM/JS
    println(crypto.hmacSha256("key", "data"))   // hex
}
```

Os valores batem com os vetores de referência (FIPS 180-4, RFC 2104) e são
idênticos nos três targets.

## AES-GCM (criptografia simétrica autenticada)

```kof
main() {
    // chave segura de 32 bytes hex
    var key = crypto.randomHex(32)
    var ct = crypto.encryptAesGcm("segredo", key)
    println(crypto.decryptAesGcm(ct, key) == "segredo")   // true
}
```

Formato: `aesgcm$<iv-b64>$<ciphertext+tag-b64>`.

**Propriedades:**
- GCM garante **confidencialidade + autenticidade** (detecta tamper).
- Chave errada ou dado alterado → falha na descriptografia.

## Aleatório seguro

```kof
var hex = crypto.randomHex(32)     // 64 chars hex
var n = crypto.randomInt(100)      // inteiro em [0, 100)
```

**Nunca** use aleatório "de adivinhação" para segurança (senhas, tokens, IVs).
`crypto.randomHex/randomInt` usam fonte criptograficamente segura.

## Programas de exemplo completos

```kof
// verificando integridade com HMAC
main() {
    var chave = "segredo-compartilhado"
    var msg = "dados importantes"
    var assinatura = crypto.hmacSha256(chave, msg)
    // quem recebe recalcula e compara em tempo constante
    println(security.constantTimeEquals(assinatura, crypto.hmacSha256(chave, msg)))
}

// round-trip AES-GCM + detecção de tamper
main() {
    var key = crypto.randomHex(32)
    var ct = crypto.encryptAesGcm("dados", key)
    var corrompido = ct.substring(0, ct.length - 4) + "XXXX"
    try {
        var dec = crypto.decryptAesGcm(corrompido, key)
        println("aceitou tamper: " + dec)     // NÃO deve chegar aqui
    } catch (String e) {
        println("tamper detectado")           // esperado
    }
}
```

## Anti-padrões

- Reutilizar IV/nonce com a mesma chave AES-GCM → catástrofe de segurança.
- Usar `crypto.sha256` como "cifra" → digest não cifra.
- Aleatório não-seguro para tokens/IVs → use `crypto.randomHex/randomInt`.

## Exercícios

1. Crie `assinatura(msg, chave)` e `verificar(msg, chave, assinatura)` com
   `hmacSha256` + `constantTimeEquals`.
2. Encripte um arquivo com AES-GCM e salve a chave num local seguro.
3. Verifique os vetores SHA-256 do FIPS 180-4 (`crypto.sha256("abc")`).
4. (Reflexão) Por que autenticação de dados (GCM/HMAC) importa além da
   confidencialidade?