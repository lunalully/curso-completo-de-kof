# Pipeline de release do Kof (do commit à release)

```
commit na main
   │
   ▼
CI (GitHub Actions)
   ├─ build: mvn clean package
   ├─ testes: mvn clean test
   ├─ bump: scripts/bump-version.sh (VERSION → pom.xml → version.properties)
   ├─ package: scripts/package.sh (tarball/zip + SHA256SUMS)
   └─ release: tag kof-<versão> + GitHub Release + changelog
```

## Fonte única de verdade
- `VERSION` na raiz do repo.
- `scripts/bump-version.sh` sincroniza VERSION → pom.xml `<revision>` →
  `kof-compiler/src/main/resources/dev/kof/version.properties`.

## Artefatos
- kof-<v>-linux-x86_64.tar.gz
- kof-<v>-windows-x86_64.zip
- kof-<v>-macos-x86_64.tar.gz
- SHA256SUMS

## Regras
- main nunca aponta para estado que não compila.
- Release só se `mvn clean test` e `mvn clean package` passarem.
- Commit de bump usa `[skip ci]` para não re-disparar a pipeline.
