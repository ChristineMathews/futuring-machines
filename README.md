# futuring machines

Interface for LLM-Human co-writing of future stories.

## Project Setup

### Requirements

- [Node.js](https://nodejs.org/en)
- [Ollama](https://ollama.com) or an API exposing a comparable `api/generate` [endpoint](https://github.com/ollama/ollama/blob/main/docs/api.md#generate-a-completion).

### Interface

install dependencies
```sh
npm install
```

compile and hot-reload for development
```sh
npm run dev
```

compile and minify for production
```sh
npm run build
```

lint with [ESLint](https://eslint.org/)
```sh
npm run lint
```

### Specify Model and API URL
Open the `.env` file and set the `VITE_MODEL` and `VITE_API_URL` environment variables accordingly. When using Ollama you'll need to pull the model before using it.

```
VITE_MODEL=mistral
VITE_API_URL=http://localhost:11434/api/generate
```

### Adding Story Templates
Story templates serve as a starting point for writers. They can be static text, text generated by a LLM, or a mix of both.

Story templates are json files located in `src/assets/templates` and imported in `src/assets/templates/index.js`.

Each story template requires a `name` and one or more `actions`. Each action requires a `type` (`static` or `generate`) and a `template`. Depending on the type the template will either just return the template text or send it to the LLM to generate the text response.

A simple template for static text:
```json
{
  "name": "static design template",
  "actions": [
    {
      "type": "static",
      "template": "In a world where technology has advanced beyond our wildest dreams, design practices have evolved to keep pace. As we step into the future, designers are no longer confined by the limitations of physical materials and two-dimensional screens."
    }
  ]
}
```

A simple template for text generation
```json
{
  "name": "generated design template",
  "actions": [
    {
      "type": "generate",
      "template": "Write the first paragraph of a story on the state of design practice in 2100."
    }
  ]
}
```

For easy customization you can specify variable in `env` and envoke them in the template string using the syntax `::variable::`.
```json
{
  "name": "generated design template",
  "actions": [
    {
      "type": "generate",
      "template": "Write the first paragraph of a story on the state of ::topic:: practice in ::year::."
    }
  ],
  "env": {
    "year": 2100,
    "topic": "design"
  }
}
```

It is also possible to chain multiple actions. The example below inserts generated text into a static template. The first action uses the `bind` property which binds the generated response to the specified variable `reply`. The static action inserts that variable by using `::reply::`
```json
{
  "name": "mixed design template",
  "actions": [
    {
      "type": "generate",
      "template": "Kim is a designer living in the year 2100. A friend asks her what she does all day. What does she respond?",
      "bind": "reply"
    },
    {
      "type": "static",
      "template": "Kim explains \"::reply:: Is this something you'd like to do as well, Alex?\""
    }
  ]
}
```



## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).