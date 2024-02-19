# Kliensalkalmazások – laborok

![Build docs](https://github.com/bmeviaubb03/laborok/workflows/Build%20docs/badge.svg?branch=master)

A [BMEVIAUBB03 – Kliensalkalmazások](https://www.aut.bme.hu/Course/VIAUBB03) tárgy laborfeladatai.

A jegyzetek MkDocs segítségével készülnek és GitHub Pagesen kerülnek publikálásra: <https://bmeviaubb03.github.io/laborok/>

Az MKDocs használatához [a hivatalos dokumentáció](https://squidfunk.github.io/mkdocs-material/creating-your-site/) a segítségedre lehet.

## Az MKDocs tesztelése (Dockerrel)

### Helyi gépen

A futtatáshoz Dockerre van szükség, amihez Windowson a [Docker Desktop](https://www.docker.com/products/docker-desktop/) egy kényelmes választás.

### GitHub Codespaces fejlesztőkörnyezetben

A GitHub Codespaces funkciója jelentős mennyiségű virtuálisgép-időt ad a felhasználók számára, ahol GitHub repositoryk tartalmát tudjuk egy virtuális gépben fordítani és futtatni.

Ehhez elegendő a repository (akár a forkon) Code gombját lenyitni, majd létrehozni egy új codespace-t. Ez lényegében egy böngészős VSCode, ami egy konténerben fut, és az alkalmazás által nyitott portokat egy port forwardinggal el is érhetjük a böngészőnkből.

### Dockerfile elindítása (Helyi gépen vagy Codespacesben)

A repository tartalmaz egy Dockerfile-t, ami az MKDocs keretrendszer és függőségeinek konfigurációját tartalmazza. Ezt a konténert le kell buildelni, majd futtatni, ami lebuildeli az MKDocs doksinkat, és egyben egy fejlesztési idejű webservert is elindít.

1. Terminál nyitása a repository gyökerébe.
2. Adjuk ki ezt a parancsot Windows (PowerShell), Linux és macOS esetén:

   ```cmd
   docker build -t mkdocs .
   docker run -it --rm -p 8000:8000 -v ${PWD}:/docs mkdocs
   ```

3. <http://localhost:8000> vagy a codespace átirányított címének megnyitása böngészőből.
4. A Markdown szerkesztése és mentése után automatikusan frissül a weboldal.
