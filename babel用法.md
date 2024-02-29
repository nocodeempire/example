```js
/**
 * @babel/core
 * babel的核心包
 * 主要有 
 * transformSync transformAsync  解析字符串到指定环境的代码
 * transformFileSync transformFileAsync  解析文件到指定环境的代码
 * transformFromAstSync transformFromAstAsync 解析Ast到指定环境的代码
 * ...等等
 */
import babel from '@babel/core'
/**
 * @babel/preset-env 是一个智能预设，允许你使用最新的 JavaScript，而无需微观管理目标环境需要哪些语法转换（以及可选的浏览器 polyfill）
 * 通俗讲就是对es6,7等往后的一些新功能, 如箭头函数, class, async等新功能, 解析到指定环境的一个集合;
 * 说智能预设是因为, 当我们设置了browserslist(browserslist能设置浏览器版本等)或者指定targets, 就能根据你指定的环境把代码解析至此环境支持的代码
 * 类似的集合有@babel/preset-typescript, @babel/preset-react等
 */
import presetEnv from '@babel/preset-env'

import parser from '@babel/parser';

// example1
const sourceCode = "const aa = () => console.log('NAME')"
const { code: code1 } = babel.transformSync(
  sourceCode,
  {presets: [presetEnv]}
  // {presets: [[presetEnv, {targets: {"chrome": "46"}}]]} // 指定47 箭头函数就还是箭头函数
);
console.log(code1)


// example2
const sourceCode1 = "const aa = () => console.log('NAME')"
const ast1 = parser.parse(sourceCode1, {})
const { code } = babel.transformFromAst(ast1, null, {
  presets: [presetEnv],
});
console.log(code);

```
