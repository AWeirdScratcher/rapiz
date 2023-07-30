# rapiz
The frontend & backend tool for everyone, fast.

## File Structure
The file structure of rapiz stays simple — APIs and Pages stay in their own directory, you can think of it as their homes.

```haskell
my-app/
├─ src/
│  ├─ api/
│  │  ├─ db.py
│  ├─ pages/
│  ├─ ├─ index.py
│  │  ├─ users/
│  │  │  ├─ [name].py
├─ rapiz.config.py
```

## Pages
Like any other tools you've seen, rapiz does pretty much the same. Define a function named `default` and rapiz will think of it as your default export.

To return a response, you can use rapiz' `html` class to do anything with html. Alternatively, you can use the `markdown` class to do everything in Markdown.

```python
from rapiz import html

async def default(ctx):
  return html.h1(
    "I love HTML!",
    style={
      "font-family": "sans-serif"
    }
  )
```

### Components & States
Manage components & states like a pro! Do everything just like what you'd do in React.

```python
from rapiz import component, html

@component
async def Button(ctx):
  [clicks, set_clicks] = ctx.use_state(1)
  return html.Button(f"Click! ({clicks})", on_click=lambda: set_clicks(clicks + 1))

async def default(ctx):
  return Button()
```

## APIs
rapiz uses FastAPI under the hood, so it's definitely easy & fast to make APIs (as its name suggests):

```python
# src/api/db.py
from fastapi import Request
from rapiz import only_us

methods = ["GET"] # assign available methods

@only_us # block external (reverse-engineered) requests
async def default(request: Request):
  try:
    data: dict = await fake_db.load()
    return data
  except ThunderStormWarning:
    return 418, {
      "message": "Go grab yourself some tea!"
    }
```

With that being said, you can easily fetch your APIs from your pages:
```python
# src/pages/index.py
from rapiz import html, fetch_api

async def default(ctx):
  data = await fetch_api("/db")
  return html.p(
    "Here are all the available services: " + data['all_services']
  )
```

## HMR
rapiz also supports HMR, it seems like HMR is a must ever since the vite era has come.

