# Módulo 09 · Aula 3 — Criptografia Aplicada

> Criptografia na prática com `kof.crypto`. O foco é **aplicar certo**, não
> implementar primitivas.

## O que cada primitiva resolve

| Primitiva | Resolve | NÃO resolve |
|-----------|---------|-------------|
| SHA-256/512 | integridade (digest) | confidencialidade |
| HMAC-SHA256 | integridade + autenticação (chave compartilhada) | confidencialidade |
| AES-GCM | confidencialidade + integridade (autenticada) | troca de chave |
| PBKDF2 | derivar chave/senha (lento de propósito) | tudo mais |
| random (seguro) | IVs, salts, tokens | — |

## Regras de ouro

1. **Hash de senha** → `passwords.hash` (PBKDF2). Nunca `sha256`.
2. **Integridade** → HMAC com chave. `sha256` sozinho é manipulável.
3. **Cifrar e autenticar** → AES-GCM (já faz os dois).
4. **IV/nonce único** por mensagem com a mesma chave.
5. **Comparações secretas** → `security.constantTimeEquals`, nunca `==`.

## Exemplos completos

### 1. Digest com verificação

```kof
main() {
    var arquivo = readFile("dados.txt")
    var hash = crypto.sha256(arquivo)
    println("sha256: " + hash)
    // para verificar integridade, compare com um hash conhecido
}
```

### 2. Autenticando dados com HMAC

```kof
main() {
    var chave = secrets.get("HMAC_KEY", "dev-key")
    var msg = "{"transferencia": 1000}"
    var mac = crypto.hmacSha256(chave, msg)

    // destino recalcula e compara em tempo constante
    var esperado = crypto.hmacSha256(chave, msg)
    println(security.constantTimeEquals(mac, esperado))   // true
}
```

### 3. Cifrando com AES-GCM (confidencialidade + integridade)

```kof
main() {
    var chave = crypto.randomHex(32)     // 256 bits, segura
    var texto = "mensagem secreta"
    var cifrado = crypto.encryptAesGcm(texto, chave)
    var decifrado = crypto.decryptAesGcm(cifrado, chave)
    println(decifrado == texto)          // true

    // tamper detection
    var corrompido = cifrado.substring(0, cifrado.length - 4) + "AAAA"
    try {
        crypto.decryptAesGcm(corrompido, chave)
        println("falhou em detectar")    // não deve acontecer
    } catch (String e) {
        println("tamper detectado")      // esperado
    }
}
```

### 4. Segredo de alta entropia para tokens

```kof
main() {
    var token = crypto.randomHex(32)     // 64 chars — seguro para tokens
    println(token.length)                // 64
}
```

## Erros comuns (ataques na prática)

| Erro | Consequência |
|------|--------------|
| Reusar IV no AES-GCM | vaza padrões; quebra confidencialidade |
| SHA-256 para senha | força bruta em GPU |
| `==` para hashes/tokens | timing attack |
| Chave fraca/hard-coded | comprometimento total |
| Digest sem autenticação | atacante altera o dado e recalcula o hash |

## Exercícios

1. Cifre/decifre uma mensagem com AES-GCM e confirme round-trip.
2. Autentique um JSON com HMAC e verifique com `constantTimeEquals`.
3. Gere um token de 64 hex e confirme entropia.
4. (Reflexão) Por que `==` é inseguro para comparar segredos? (pense em
   timing)