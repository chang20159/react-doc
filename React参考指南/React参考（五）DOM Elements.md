>[DOM Elements](https://facebook.github.io/react/docs/dom-elements.html)

# DOM元素
React实现了与浏览器独立的DOM系统，以实现性能和跨浏览器兼容性。		
在React中，所有DOM属性（包括事件处理程序）都得是驼峰式命名。 	
例如，HTML属性tabindex对应于React中的属性tabIndex。 	
除了aria-*和data- *属性必须是小写。 例如，aria-label在React中还是aria-label。

## React与HTML的DOM属性差异
有一些属性在React与在HTML中有些不同：
### checked
**checked**属性在checkbox或radio类型的&lt;input>组件中使用，可以使用它来设置是否选中组件，这对于构建受控组件很有用。 **defaultChecked**属性不具有控制性，用语在组件首次挂载时设置是否选中。
### className
在React中要指定一个CSS类，请使用**className**属性。 这适用于所有常规的DOM和SVG元素，如&lt;div>，&lt;a>和其他。

如果在React中使用 [Web Components](../React高级指南/React高级指南（十）Web Components.md)（这不常见），请改用class属性。
### dangerouslySetInnerHTML
**dangerlySetInnerHTML**是相当于在浏览器DOM中使用innerHTML。 一般来说，在代码中设置HTML是有风险的，因为很容易无意中将用户暴露在跨站点脚本（XSS）攻击中。您可以直接设置HTML，但是必须给dangerouslySetInnerHTML传递一个带有__html键的对象，以提醒自己这是危险的。 例如：

```javascript
function createMarkup() {
  return {__html: 'First &middot; Second'};
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}
```

### htmlFor
由于for是JavaScript中的保留字，React元素使用**htmlFor**代替。
### onChange
每当更改表单字段时，将触发onChange事件。 我们特意不使用现有的浏览器行为，因为onChange不能正确处理浏览器行为，而React依赖此事件来实时处理用户输入。
### selected#
**selected**属性由&lt;option>组件支持，可以使用它来设置是否选择组件。 这对于构建受控组件很有用。
### style
**style属性接收一个JavaScript对象**，它的属性必须是驼峰式命名而不是CSS字符串。 这与DOM风格的JavaScript属性一致，效率更高，并且可以防止XSS安全漏洞。 例如：

```javascript
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```
请注意，style不会自动添加前缀， 要支持旧版浏览器，您需要提供相应的样式属性：

```javascript
const divStyle = {
  WebkitTransition: 'all', // 注意这里的首字母 'W' 是大写的
  msTransition: 'all' // 'ms'是唯一的小写字母供应商前缀
};

function ComponentWithTransition() {
  return <div style={divStyle}>This should work cross-browser</div>;
}
```
style对象的key必须是驼峰式的，以便与使用JS访问DOM节点上的属性（例如node.style.backgroundImage）保持一致。

**除ms之外的[供应商前缀](http://www.andismith.com/blog/2012/02/modernizr-prefixed/)应以大写字母开头**。 这就是为什么WebkitTransition有一个大写“W”的原因。
### suppressContentEditableWarning
通常情况下，当有child的元素也被标记为contentEditable时，会出现警告，因为它不起作用。 设置**suppressContentEditableWarning**属性会禁止该警告。 除非您正在构建一个可以手动管理contentEditable的 [Draft.js](https://draftjs.org/) 的库，否则不要使用此属性。

### value
**value**属性由&lt;input>和&lt;textarea>组件支持。 您可以使用它来设置组件的值,这对于构建受控组件很有用。 **defaultValue**对组件不具有控制性，它在首次挂载时设置组件的值。
## 所有支持的HTML属性
React支持所有data- *和aria- *属性以及这些属性：

```
accept acceptCharset accessKey action allowFullScreen allowTransparency alt
async autoComplete autoFocus autoPlay capture cellPadding cellSpacing challenge
charSet checked cite classID className colSpan cols content contentEditable
contextMenu controls coords crossOrigin data dateTime default defer dir
disabled download draggable encType form formAction formEncType formMethod
formNoValidate formTarget frameBorder headers height hidden high href hrefLang
htmlFor httpEquiv icon id inputMode integrity is keyParams keyType kind label
lang list loop low manifest marginHeight marginWidth max maxLength media
mediaGroup method min minLength multiple muted name noValidate nonce open
optimum pattern placeholder poster preload profile radioGroup readOnly rel
required reversed role rowSpan rows sandbox scope scoped scrolling seamless
selected shape size sizes span spellCheck src srcDoc srcLang srcSet start step
style summary tabIndex target title type useMap value width wmode wrap
```

也支持这些RDFa属性（有几个RDFa属性与标准HTML属性重叠，因此从此列表中排除）：

```
about datatype inlist prefix property resource typeof vocab
```

此外，还支持以下非标准属性：

- autoCapitalize autoCorrect (Mobile Safari)
-  &lt;link rel="mask-icon" />  color属性 （Safari）
-  [HTML5 microdata](http://schema.org/docs/gs.html) 中的itemProp itemScope itemType itemRef itemID
-  旧版本IE浏览器的security
-  IE浏览器的unselectable
-  type为search的input中results 和 autoSave属性（ WebKit/Blink）

## 所有支持的SVG属性
React支持这些SVG属性：

```
accentHeight accumulate additive alignmentBaseline allowReorder alphabetic
amplitude arabicForm ascent attributeName attributeType autoReverse azimuth
baseFrequency baseProfile baselineShift bbox begin bias by calcMode capHeight
clip clipPath clipPathUnits clipRule colorInterpolation
colorInterpolationFilters colorProfile colorRendering contentScriptType
contentStyleType cursor cx cy d decelerate descent diffuseConstant direction
display divisor dominantBaseline dur dx dy edgeMode elevation enableBackground
end exponent externalResourcesRequired fill fillOpacity fillRule filter
filterRes filterUnits floodColor floodOpacity focusable fontFamily fontSize
fontSizeAdjust fontStretch fontStyle fontVariant fontWeight format from fx fy
g1 g2 glyphName glyphOrientationHorizontal glyphOrientationVertical glyphRef
gradientTransform gradientUnits hanging horizAdvX horizOriginX ideographic
imageRendering in in2 intercept k k1 k2 k3 k4 kernelMatrix kernelUnitLength
kerning keyPoints keySplines keyTimes lengthAdjust letterSpacing lightingColor
limitingConeAngle local markerEnd markerHeight markerMid markerStart
markerUnits markerWidth mask maskContentUnits maskUnits mathematical mode
numOctaves offset opacity operator order orient orientation origin overflow
overlinePosition overlineThickness paintOrder panose1 pathLength
patternContentUnits patternTransform patternUnits pointerEvents points
pointsAtX pointsAtY pointsAtZ preserveAlpha preserveAspectRatio primitiveUnits
r radius refX refY renderingIntent repeatCount repeatDur requiredExtensions
requiredFeatures restart result rotate rx ry scale seed shapeRendering slope
spacing specularConstant specularExponent speed spreadMethod startOffset
stdDeviation stemh stemv stitchTiles stopColor stopOpacity
strikethroughPosition strikethroughThickness string stroke strokeDasharray
strokeDashoffset strokeLinecap strokeLinejoin strokeMiterlimit strokeOpacity
strokeWidth surfaceScale systemLanguage tableValues targetX targetY textAnchor
textDecoration textLength textRendering to transform u1 u2 underlinePosition
underlineThickness unicode unicodeBidi unicodeRange unitsPerEm vAlphabetic
vHanging vIdeographic vMathematical values vectorEffect version vertAdvY
vertOriginX vertOriginY viewBox viewTarget visibility widths wordSpacing
writingMode x x1 x2 xChannelSelector xHeight xlinkActuate xlinkArcrole
xlinkHref xlinkRole xlinkShow xlinkTitle xlinkType xmlns xmlnsXlink xmlBase
xmlLang xmlSpace y y1 y2 yChannelSelector z zoomAndPan
```