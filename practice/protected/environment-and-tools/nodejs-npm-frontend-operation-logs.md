
# package.json
在package.json中，我们通常提供了默认的项目开发中所需要的一些命令，例如：
```json
  "scripts": {
    "dev": "vue-cli-service serve",
    "build:prod": "vue-cli-service build",
    "build:stage": "vue-cli-service build --mode staging",
    "preview": "node build/index.js --preview",
    "lint": "eslint --ext .js,.vue src",
    "test:unit": "jest --clearCache && vue-cli-service test:unit",
    "test:ci": "npm run lint && npm run test:unit",
    "svgo": "svgo -f src/icons/svg --config=src/icons/svgo.yml"
  }
```

你可以通过 npm 或 Yarn 调用这些 script
```bash
npm run serve
# OR
yarn serve
```

如果你可以使用 `npx` (最新版的 `npm` 应该已经自带)，也可以直接这样调用命令：
```
npx vue-cli-service serve
```
