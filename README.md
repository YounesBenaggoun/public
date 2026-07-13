nodemon.json

```
{
    "watch": [
        "src"
    ],
    "ext": "js,json,ts",
    "ignore": [
        "src/**/*.test.js",
        "node_modules"
    ],
    "exec": "node src/server.js",
    "delay": "500",
    "verbose": true
}
```

eslint.config.js

```
import js from "@eslint/js";
import globals from "globals";

export default [
  js.configs.recommended,
  {
    languageOptions: {
      globals: globals.node,
    },
  },
  {
    rules: {
      "no-unused-vars": [
        "error",
        {
          argsIgnorePattern: "^_",
        },
      ],
    },
  }
];

```
