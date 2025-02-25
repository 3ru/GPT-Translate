# 🌎 BOT de Traducción de Markdown
[![Mantenibilidad](https://api.codeclimate.com/v1/badges/a13ea4f37913ba6ba570/maintainability)](https://codeclimate.com/github/3ru/gpt-translate/maintainability)
[![GPT Translate](https://github.com/3ru/gpt-translate/actions/workflows/gpt-translate.yml/badge.svg)](https://github.com/3ru/gpt-translate/actions/workflows/gpt-translate.yml)

Esta acción de GitHub traduce tus archivos de markdown a múltiples idiomas utilizando el modelo GPT-3.5.

### ⚠️**Advertencia**
La API de OpenAI no está actualmente disponible de forma gratuita. Necesitas una clave de API emitida con una `cuenta de pago` para usar este flujo de trabajo.  
<img width="387" alt="image" src="https://github.com/3ru/gpt-translate/assets/69892552/8c803edb-85ef-41ee-a4be-be52b3a30eba">

<br/>

<details><summary> Resumen de este README por GPT-4</summary>
<p>

> - Esta es una acción de GitHub que utiliza GPT-3.5 para traducir archivos de markdown a múltiples idiomas.
> - Para usarla, crea un comentario con /gpt-translate o /gt en un issue o pull request, especificando las rutas de entrada/salida de archivo y el idioma objetivo.
> - Los archivos traducidos se crearán como un pull request (en issues) o se agregarán al pull request existente como un nuevo commit (en pull requests).
</p>
</details> 

<details><summary>🧐 Estado actual</summary>
<p>

- La acción admite la traducción solo de **archivos de markdown individuales**.

- El comando puede ser ejecutado exclusivamente por personas con **permisos de escritura en el repositorio**.

Estas limitaciones evitan el abuso de la API por parte de partes no confiables.

Estoy considerando la implementación de características de selección múltiple y traducción por directorio para futuras implementaciones.
</p>
</details> 

## 🔧 Configuración

### Configuración del Repositorio

#### 1. Configuración > Acciones > General

- Habilita `Permisos de lectura y escritura`
- Habilita `Permitir que las acciones de GitHub creen y aprueben pull requests`
  ![permissions](https://user-images.githubusercontent.com/69892552/228692074-d8d009a8-9272-4023-97b1-3cbc637d5d84.jpg)

#### 2. Configuración > Secretos y variables > Acciones

- Establece [tu clave de API](https://platform.openai.com/account/api-keys)(`OPENAI_API_KEY`) como secretos
  ![secrets](https://user-images.githubusercontent.com/69892552/228692421-22d7db33-4e32-4f28-b166-45b4d3ce2b11.jpg)


### Configuración del Flujo de Trabajo de GitHub Actions

#### Requerido
- Proporciona la OPENAI_API_KEY como apiKey.
- Establece `on` para que se active cuando se crea un comentario (`types: [ created ]`).
- Checkout de antemano(`actions/checkout@v3`).

#### Recomendado (Para minimizar el tiempo de ejecución innecesario)
- Configura si se ejecuta solo cuando `/gpt-translate` o `/gt` está presente en el comentario.


👇 Aquí tienes un ejemplo mínimo de flujo de trabajo:
```yaml
# .github/workflows/gpt-translate.yml
name: GPT Translate

on:
  issue_comment:
    types: [ created ]

jobs:
  gpt_translate:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Ejecutar GPT Translate
        if: |
          contains(github.event.comment.body, '/gpt-translate') || 
          contains(github.event.comment.body, '/gt')
        uses: 3ru/gpt-translate@v1.0
        with:
          apikey: ${{ secrets.OPENAI_API_KEY }}
```


## 💡 Uso

```
/gpt-translate [ruta de archivo de entrada] [ruta de archivo de salida] [idioma objetivo] 
```
Puedes usar /gt como abreviatura de /gpt-translate.

1.Crea un comentario con `/gpt-translate` o `/gt` en un issue o pull request.

2.【En issue】Los archivos traducidos se crearán como un **pull request**.

2.【En pull request】Los archivos traducidos se **agregarán al pull request con un nuevo commit**.

En otras palabras, si sigues comentando en un issue, se crearán continuamente nuevos PR.
Si sigues comentando en un PR, se agregarán continuamente nuevos commits a ese PR.

## 🌐 Idiomas Soportados
Cualquier idioma interpretado por GPT-3.5

## 📃 Licencia
Licencia MIT