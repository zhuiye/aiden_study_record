# H5 实现 签名业务,并生成 pdf 文件

## 前言

近期由于业务需要,需要实现一个 H5 网页在线签名的业务,具体流程如下:

网页展示协议内容->定位到签名板->用户签名->生成 PDF->上传至服务器端保存

本以为一切会很简单,但没想到过程极为曲折, html2canvas 尽管是一个不错的库,但
坑也比较多

## 实现方案:

遍历 DOM 节点,生成图片,可借助[html2Canvas](https://github.com/niklasvh/html2canvas)库 ,最后借用[jsPDF](https://github.com/MrRio/jsPDF)生成 PDF 文件即可,下列列出实现方案的组合,我选择了 html2pdf.js

1. [react-signature-canvas](https://github.com/agilgur5/react-signature-canvas) + [html2pdf.js(^0.9.2")](https://github.com/eKoopmans/html2pdf.js)
2. [react-signature-canvas](https://github.com/agilgur5/react-signature-canvas) + [html2canvas](https://github.com/eKoopmans/html2pdf.js) + [jsPDF](https://github.com/MrRio/jsPDF)

## 采用 html2pdf.js 实现本次需求

### html2pdf.js 的主要配置

```ts
import html2pdf from "html2pdf.js";

const element = document.getElementById("pdf");

let opt = {
  filename: "demo.pdf",
  image: { type: "jpeg", quality: 0.6 },
  html2canvas: {
    useCORS: true, //要渲染图片,此项必须要有
    backgroundColor: null,
    scrollX: 0, // 如果不这样设置,生成的PDF只有可视区的内容
    scrollY: 0, // 如果不这样设置,生成的PDF只有可视区的内容
    scale: 1, // 用于渲染的尺度,默认设备的像素比 window.devicePixelRatio
  },
  // before: 在 .break-page 之前分隔 ,after: 在 #after1,#after2 之后分割
  pagebreak: { before: ".break-page", after: ["#after1", "#after2"] }, // 控制分页符
  jsPDF: { orientation: "portrait", compressPDF: true }, // unit: 'pt', format: 'a4', // 此库中的一些配置
};

html2pdf()
  .set(opt)
  .from(element)
  .toPdf()
  .get("pdf")
  .then(async function (pdfObj) {
    // 得到的文件perBlob 用于 上传服务器端
    const perBlob = pdfObj.output("blob");
    const formData = new FormData();
    /* https://stackoverflow.com/questions/27159179/how-to-convert-blob-to-file-in-javascript
     */
    const file = new File([perBlob], "filename.pdf", { type: "pdf" });
    formData.append("file", file, opt.filename);
    uploadFile(formData);
  });
// 直接下载 到设备
html2pdf().set(opt).from(element).save();
```

### 生成的 PDF 文件中忽略某元素

某些元素,我们并不需要渲染在 PDF 中,我们可以给其设置 `data-html2canvas-ignore="true" ` 的自定义属性, 如下:

```html
<div data-html2canvas-ignore="true">
  设置此自定义属性的标签,将不会在生成PDF中显示
</div>
```

或者配置 html2canvas 的 ignoreElements

```ts
   {
     ignoreElements	(element) =>{
       // 返回 true ,则过滤改元素
       return element.className.toString().indexOf("ignore")>-1
     }
   }
```

### 生成的 PDF 发送至 服务器端

可参考如下链接:

https://github.com/eKoopmans/html2pdf.js/issues/272

https://github.com/eKoopmans/html2pdf.js/issues/271

https://github.com/MrRio/jsPDF/issues/771

http://raw.githack.com/MrRio/jsPDF/master/docs/jsPDF.html#output

```ts
const file = new File([doc.output("blob")], "filename.pdf", { type: "pdf" });
const data = new FormData();
data.append("file", file);
axios
  .post("/amazing-endpoint", data)
  .then((res) => console.log(res))
  .catch((err) => console.log(err));
```

## 坑点

若单独使用 html2canvas,会遇到的坑点

### 内容渲染不全的坑

- https://stackoverflow.com/questions/36213275/html2canvas-does-not-render-full-div-only-what-is-visible-on-screen

- https://github.com/niklasvh/html2canvas/issues/1878

总有一个方案能解决......下面贴一个

```ts
window.scrollTo(0, 0);
html2canvas(element as any, {
  useCORS: true,
  scale: 1, // 如果不设置为1,会显示一大片空白....,怪
}).then(async (canvas) => {
  window.scrollTo(
    0,
    document.body.scrollHeight || document.documentElement.scrollHeight
  );
});
```

### 关于 html2canvas 的一篇文章

https://zhuanlan.zhihu.com/p/128935733
