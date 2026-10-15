voila 


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
installation commitLint and husky (si les tests n'existe pas il passe

```
#!/bin/bash
set -e

echo "📦 Installation des dépendances..."
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional

echo "🐶 Initialisation de Husky..."
npx husky init

echo "🔧 Configuration du hook commit-msg..."
echo 'npx --no -- commitlint --edit "$1"' > .husky/commit-msg

echo "🧪 Configuration du hook pre-commit..."
cat > .husky/pre-commit << 'EOF'
#!/usr/bin/env sh

TEST_SCRIPT=$(node -p "require('./package.json').scripts?.test || ''")

if [ -z "$TEST_SCRIPT" ] || echo "$TEST_SCRIPT" | grep -q "no test specified"; then
  echo "⏭️  Aucun test configuré, on passe."
  exit 0
fi

npm test
EOF

echo "📝 Création de commitlint.config.js (ESM)..."
cat > commitlint.config.js << 'EOF'
export default {
  extends: ['@commitlint/config-conventional'],
};
EOF

echo "⚙️  Configuration du script prepare..."
npm pkg set scripts.prepare="husky"

echo ""
echo "✅ Installation terminée !"
echo "  • commit-msg  → vérifie toujours le message de commit"
echo "  • pre-commit  → lance les tests SEULEMENT s'ils existent"

```
