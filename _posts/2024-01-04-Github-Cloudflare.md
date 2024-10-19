---
layout: single
title: Implementando Github Pages con Cloudflare
excerpt: "Se trata de un pequeño manual donde se explica como obtener un sitio web personal implementando Github y Cloudflare."
date: 2024-10-12
classes: wide
header:
  teaser: /assets/images/Github.Pages/Pages.png
  teaser_home_page: true
categories:
  - Sistema Operativo
tags:
  - Código Abierto
---

<p align="center">
<img src="/assets/images/Github.pages/Portada.png">
</p>

[GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) está disponible en repositorios públicos con *Github Free* y *Github Free* para organizaciones, y en repositorios públicos y privados con *GitHub Pro*, *GitHub Team*, *GitHub Enterprise Cloud* y *GitHub Enterprise Server*. Para obtener más información ir a [Planes de Github](https://docs.github.com/en/get-started/learning-about-github/githubs-plans).

GitHub Pages ahora utiliza `GitHub Actions` para ejecutar la compilación de ***Jekyll***. Cuando se utiliza una rama como fuente de la compilación, *GitHub Actions* debe estar habilitado en el repositorio si se desea utilizar el flujo de trabajo integrado de Jekyll. De manera alternativa, si GitHub Actions no está disponible o está deshabilitado, agregar un archivo `.nojekyll` a la raíz de la rama de origen omitirá el proceso de compilación de *Jekyll* e implementará el contenido directamente. Para obtener más información sobre cómo habilitar **GitHub Actions**, consulte [Administrar GitHub Actions](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository) para un repositorio.

## Administrar un dominio personalizado
Puede configurar o actualizar ciertos *registros DNS* y la configuración de su repositorio para apuntar el dominio predeterminado de su sitio de GitHub Pages a un *dominio personalizado*.

```text
Consejo.

  Se recomienda verificar el el dominio personalizado antes de agregarlo a su repositorio para mejorar la seguridad y evitar ataques de apropiación.
```

Para obtener más información, consulte: [Verificación de dominio personalizado](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages) para páginas de Github.

Asegúrate de agregar tu dominio personalizado a tu sitio de *GitHub Pages* antes de configurarlo con tu proveedor de DNS. Si lo configuras con tu proveedor de DNS sin agregarlo a GitHub, es posible que otra persona pueda alojar un sitio en uno de tus subdominios.

```text
Nota:
  Los cambios de DNS pueden tardar hasta 24 horas en propagarse.
```

### Configurar Dominio de ápice
Para configurar un *dominio de ápice*, como `example.com`, debe configurar un dominio personalizado en la configuración del repositorio y al menos un registro `ALIAS`, `ANAME` ó `A` con su proveedor de DNS.

1. En *GitHub*, navega hasta el repositorio de tu sitio.

2. Debajo del nombre de su repositorio, haga clic en `Configuración`. Si no puede ver la pestaña *"Configuración"*, seleccione el `*** menú desplegable` y luego haga clic en *Configuración*.

3. En la sección *Código y automatización* de la barra lateral, haga clic en `Páginas`.

4. En *"Dominio Personalizado"*, escribe el dominio personalizado y haz clic en `Guardar`. Si se está publicando el sitio desde una rama, esto creará una confirmación que agrega un archivo `CNAME` directamente a la raíz de tu rama de origen. Si está pubblicando el sitio con un flujo de trabajo de GitHub Actions personalizado, no se crea ningún archivo `CNAME`, por lo que se debe crear manualmente (que contenga solo una línea de texto con el dominio personalizado). Para obtener más información sobre la fuente de publicación, consulta: [Configura fuente de publicación](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) para un sitio de GitHub Pages.

5. Vaya a su proveedor de DNS y cree un registro `ALIAS`, `ANAME` ó `A`. También se pueden crear registros `AAAA` para la compatibilidad con *IPv6*. Si está implementando compatibilidad con IPv6, se recomienda encarecidamente que utilice un registro `A` además de los registros `AAAA`, debido a la lenta adopción de IPv6 a nivel mundial. Para obtener más información sobre cómo crear el registro correcto, consulte la documentación de su proveedor de DNS.

- Para crear un registro `ALIAS` o `ANAME`, apunte su dominio de Apex al dominio predeterminado de su sitio. Para obtener más información sobre el dominio predeterminado de su sitio, consulte: [Acerca de GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites).

- Para crear registros `A`, apunte su dominio Apex a las direcciones IP de las páginas de Github.

```Text
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

- Para crear registros `AAAA`, apunte su dominio Apex a las direcciones IP de las páginas de GitHub.

```Text
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

**Advertencia:** Se recomienda encarecidamente que no se utilice registros DNS (comodín); ejemplo, `*.example.com`. Estos registros ponen en riesgo inmediato de que alguien se apodere de su dominio, incluso si se verifica. Por ejemplo, si se verifica `example.com`, esto evita que alguien use `a.example.com`, pero aún podría apoderarse de `b.a.example.com` (lo que está cubierto por el registro DNS comodín). Para obtener más información, consulta: [Verificar dominio personalizado](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages) para GitHub Pages.

6. Abrir terminal.

7. Para confirmar que su registro DNS está configurado correctamente, use el comando `dig example.com`. Confirme que los resultados coincidan con las direcciones IP de las páginas de GitHub mencionadas anteriormente.

- Para los registros `A` consta:

```text
$ dig example.com +noall +answer -t A
> example.com   3600   in A   185.199.108.153
> example.com   3600   in A   185.199.109.153
> example.com   3600   in A   185.199.110.153
> example.com   3600   in A   185.199.111.153
```

- Para los registros `AAAA` consta:

```text
$ dig example.com +noall +answer -t AAAA
> example.com   3600   in AAAA  2606:50c0:8000::153
> example.com   3600   in AAAA  2606:50c0:8001::153
> example.com   3600   in AAAA  2606:50c0:8002::153
> example.com   3600   in AAAA  2606:50c0:8003::153
```

Recuerde revisar su historial `A`

8. Si utiliza un generador de sitios estáticos para crear su sitio localmente y enviar los archivos generados a GitHub, extraiga la confirmación que agregó el archivo CNAME a su repositorio local. Para obtener más información, consulte: [Solución de problemas](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages#cname-errors) de dominios personalizados y páginas de GitHub.

9. De manera opcional, para aplicar el cifrado HTTPS en el sitio, selecciona `Aplicar HTTPS`. Esta opción puede tardar hasta 24 horas en estar disponible. Para obtener más información, consulta: [Cómo proteger el sitio con HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https).

### Configurar Dominio de vértice y la variante de subdominio (www)

```text
Nota:

  Se recomienda configurar un subdominio (www) junto con un dominio para sitios web protegidos mediante HTTPS.
```

Si usa un dominio Apex como dominio personalizado, se recomienda que también se configure un subdominio `www`. Si se configuran los registros correctos para cada tipo de dominio a través de su proveedor de DNS, GitHub Pages creará automaticamente redirecciones entre los dominios.

Por ejemplo, si se configura `www.example.com` como el dominio personalizado para el sitio y tiene registros DNS de Github Pages configurados para los dominios Apex y `www`, entonces `example.com` se redireccionará a `www.example.com`. Tenga en cuenta que las redirecciones automáticas solo se aplican al subdominio `www`. Las redirecciones automáticas no se aplican a ningún otro subdominio, como `blog`. Para obtener más información, consulta: [Configurar un subdominio](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site?platform=linux#configuring-a-subdomain).

Navega hasta tu proveedor de DNS y crea un registro `CNAME` para el subdominio `www` que apunte a tu dominio predeterminado de Github Pages. Por ejemplo, si tu sitio está ubicado en `<user>.github.io`, debes crear un registro `CNAME` que apunte de `www.example.com` a `<user>.github.io`. De manera similar, para un sitio de una organización ubicado en `<organization>.github.io`, debes crear un registro `CNAME` que apunte de `www.example.com` a `<organization>.github.io`. Asegúrate de que el registro `CNAME` apunte directamente a `<user>.github.io` ó `<organization>.github.io` sin incluir el nombre del repositorio.

Para obtener más información sobre cómo crear el registro correcto, consulta la documentación de tu proveedor de DNS. Para obtener más información sobre el dominio predeterminado de tu sitio, consulta: [Acerca de Github Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites).

### Configurar Subdominio
Para configurar un subdominio personalizado `www`, como `www.example.com` ó `blog.example.com`, debe agregar su dominio en la configuración del repositorio. Luego configure un registro `CNAME` con su proveedor de DNS.

1. En *GitHub*, navega hasta el repositorio de tu sitio.

2. Debajo del nombre de su repositorio, haga clic en `Configuración`. Si no puede ver la pestaña *"Configuración"*, seleccione el `*** menú desplegable` y luego haga clic en *Configuración*.

3. En la sección *Código y automatización* de la barra lateral, haga clic en `Páginas`.

4. En *"Dominio Personalizado"*, escribe el dominio personalizado y haz clic en `Guardar`. Si se está publicando el sitio desde una rama, esto creará una confirmación que agrega un archivo `CNAME` directamente a la raíz de tu rama de origen. Si está pubblicando el sitio con un flujo de trabajo de GitHub Actions personalizado, no se crea ningún archivo `CNAME`, por lo que se debe crear manualmente (que contenga solo una línea de texto con el dominio personalizado). Para obtener más información sobre la fuente de publicación, consulta: [Configura fuente de publicación](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) para un sitio de GitHub Pages.

**Nota:** Si su dominio personalizado es un nombre de dominio internacionalizado, debe ingresar la versión codificada en Punycode. Para obtener más información de Punycodes, consulta: [Nombre de dominio Internacionalizado](https://en.wikipedia.org/wiki/Internationalized_domain_name).

5. Navega hasta tu proveedor de DNS y crea un registro `CNAME` que apunte tu subdominio al dominio predeterminado de tu sitio. Por ejemplo, si quieres usar el subdominio `www.example.com` para tu sitio de usuario, crea un registro `CNAME` que apunte de `www.example.com` a `<user>.github.io`. Si quieres usar el subdominio `another.example.com` para el sitio de tu organización, crea un registro `CNAME` que apunte de `another.example.com` a `<organization>.github.io`.

El registro `CNAME` siempre debe apuntar a `<user>.github.io` ó `<organization>.github.io`, excluyendo el nombre del repositorio. Para obtener más información sobre cómo crear el registro correcto, consulta la documentación de tu proveedor de DNS. Para obtener más información sobre el dominio predeterminado de tu sitio, consulta: [Acerca de GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites).

**Advertencia:** Se recomienda encarecidamente que no se utilice registros DNS (comodín); ejemplo, `*.example.com`. Estos registros ponen en riesgo inmediato de que alguien se apodere de su dominio, incluso si se verifica. Por ejemplo, si se verifica `example.com`, esto evita que alguien use `a.example.com`, pero aún podría apoderarse de `b.a.example.com` (lo que está cubierto por el registro DNS comodín). Para obtener más información, consulta: [Verificar dominio personalizado](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages) para GitHub Pages.

6. Abrir terminal.

7. Para confirmar que su registro DNS está configurado correctamente, use el comando `dig www.example.com`.

```text
$ dig www.example.com +nostats +nocomments +nocmd
> www.example.com          in A
> www.example.com   3592   in CNAME   Your-Username.github.io
> Your-Username.github.io   43192   in CNAME   Github-Pages-Server
> Github-Pages-Server    22   in A   192.0.2.1
```

8. Si utiliza un generador de sitios estáticos para crear su sitio localmente y enviar los archivos generados a GitHub, extraiga la confirmación que agregó el archivo CNAME a su repositorio local. Para obtener más información, consulte: [Solución de problemas](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages#cname-errors) de dominios personalizados y páginas de GitHub.

9. De manera opcional, para aplicar el cifrado HTTPS en el sitio, selecciona `Aplicar HTTPS`. Esta opción puede tardar hasta 24 horas en estar disponible. Para obtener más información, consulta: [Cómo proteger el sitio con HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https).

**Nota:** Si apunta su subdominio personalizado a su dominio Apex, tendrá problemas para aplicar HTTPS a su sitio web y puede tener problemas en los que su subdominio no llegue a su sitio de Github Pages en absoluto.

### Registro DNS para Dominio personalizado
