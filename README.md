
> https://blog.csdn.net/m0_61662775/article/details/130781258
## 1. 运行项目

```shell
npm run dev
```

## 2. 项目配置

### 1. 浏览器自动打开

```json
    "scripts": {
      "dev": "vite --open",
      "build": "vue-tsc && vite build",
      "preview": "vite preview"
   },
```

### 2. eslint校验 配置

eslint: 插件化检测 javascript 代码规范性测试工具

1. 安装 eslint 插件

```shell
npm install eslint
```

2. 生成 eslint 配置文件 `.eslintrc.cjs`

```shell
npx eslint --init
```

3. 安装Vue3环境代码校验插件

```json
# 让所有与prettier规则存在冲突的Eslint rules失效，并使用prettier进行代码检查
"eslint-config-prettier": "^8.6.0",
"eslint-plugin-import": "^2.27.5",
"eslint-plugin-node": "^11.1.0",
# 运行更漂亮的Eslint，使prettier规则优先级更高，Eslint优先级低
"eslint-plugin-prettier": "^4.2.1",
# vue.js的Eslint插件（查找vue语法错误，发现错误指令，查找违规风格指南
"eslint-plugin-vue": "^9.9.0",
# 该解析器允许使用Eslint校验所有babel code
"@babel/eslint-parser": "^7.19.1",
```

```shell
npm install -D eslint-plugin-import eslint-plugin-vue eslint-plugin-node eslint-plugin-prettier eslint-config-prettier eslint-plugin-node @babel/eslint-parser
```

4. eslintignore忽略文件

```
dist
node_modules
```

5. 运行脚本
   package.json新增两个运行脚本

```json
"scripts": {
    "lint": "eslint src",
    "fix": "eslint src --fix",
}
```

### 3. prettier格式化工具

1. 安装依赖

```shell 
npm install -D eslint-plugin-prettier prettier eslint-config-prettier
```
2. 创建`.prettierrc.json`文件配置规则

```json
{
  "singleQuote": true, //字符串全为单引号
  "semi": false,//无需分号
  "bracketSpacing": true,
  "htmlWhitespaceSensitivity": "ignore",
  "endOfLine": "auto",
  "trailingComma": "all",
  "tabWidth": 2
}
```

3. 配置prettier忽略文件

```
/dist/*
/html/*
.local
/node_modules/**
**/*.svg
**/*.sh
/public/*
```

通过 `npm run lint` 去检测语法，如果出现不规范格式,通过 `npm run fix` 修改

### 4. stylelint css格式化配置

stylelint 为 css的lint工具。可格式化css代码，检查css语法错误与不合理的写法，指定css书写顺序等。

项目中使用scss作为预处理器，安装以下依赖：

1. 安装依赖
```shell
npm add sass sass-loader stylelint postcss postcss-scss postcss-html stylelint-config-prettier stylelint-config-recess-order stylelint-config-recommended-scss stylelint-config-standard stylelint-config-standard-vue stylelint-scss stylelint-order stylelint-config-standard-scss -D
```


2. .stylelintrc.cjs配置文件

// @see https://stylelint.bootcss.com/
``` cjs
module.exports = {
  extends: [
    'stylelint-config-standard', // 配置stylelint拓展插件
    'stylelint-config-html/vue', // 配置 vue 中 template 样式格式化
    'stylelint-config-standard-scss', // 配置stylelint scss插件
    'stylelint-config-recommended-vue/scss', // 配置 vue 中 scss 样式格式化
    'stylelint-config-recess-order', // 配置stylelint css属性书写顺序插件,
    'stylelint-config-prettier', // 配置stylelint和prettier兼容
  ],
  overrides: [
    {
      files: ['**/*.(scss|css|vue|html)'],
      customSyntax: 'postcss-scss',
    },
    {
      files: ['**/*.(html|vue)'],
      customSyntax: 'postcss-html',
    },
  ],
  ignoreFiles: [
    '**/*.js',
    '**/*.jsx',
    '**/*.tsx',
    '**/*.ts',
    '**/*.json',
    '**/*.md',
    '**/*.yaml',
  ],
  /**
   * null  => 关闭该规则
   * always => 必须
   */
  rules: {
    'value-keyword-case': null, // 在 css 中使用 v-bind，不报错
    'no-descending-specificity': null, // 禁止在具有较高优先级的选择器后出现被其覆盖的较低优先级的选择器
    'function-url-quotes': 'always', // 要求或禁止 URL 的引号 "always(必须加上引号)"|"never(没有引号)"
    'no-empty-source': null, // 关闭禁止空源码
    'selector-class-pattern': null, // 关闭强制选择器类名的格式
    'property-no-unknown': null, // 禁止未知的属性(true 为不允许)
    'block-opening-brace-space-before': 'always', //大括号之前必须有一个空格或不能有空白符
    'value-no-vendor-prefix': null, // 关闭 属性值前缀 --webkit-box
    'property-no-vendor-prefix': null, // 关闭 属性前缀 -webkit-mask
    'selector-pseudo-class-no-unknown': [
      // 不允许未知的选择器
      true,
      {
        ignorePseudoClasses: ['global', 'v-deep', 'deep'], // 忽略属性，修改element默认样式的时候能使用到
      },
    ],
  },
}

```


3. .stylelintignore忽略文件

```
/node_modules/*
/dist/*
/html/*
/public/*
```

4. 添加运行脚本

```json
"scripts": {
    "lint:style": "stylelint src/**/*.{css,scss,vue} --cache --fix"
}
```

5. 最后配置统一的prettier来格式化我们的js和css，html代码
```json
 "scripts": {
    "dev": "vite --open",
    "build": "vue-tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src",
    "fix": "eslint src --fix",
    "format": "prettier --write \"./**/*.{html,vue,ts,js,json,md}\"",
    "lint:eslint": "eslint src/**/*.{ts,vue} --cache --fix",
    "lint:style": "stylelint src/**/*.{css,scss,vue} --cache --fix"
  },
```
当我们运行 pnpm run format 的时候，会把代码直接格式化。