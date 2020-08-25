# H5 实现 签名业务

## optional library

- [jSignature](https://github.com/willowsystems/jSignature)
- [react-signature-canvas](https://github.com/agilgur5/react-signature-canvas)
- [jsPDF](https://github.com/MrRio/jsPDF)
- [html2canvas](https://github.com/niklasvh/html2canvas)

<https://github.com/eKoopmans/html2pdf.js>
https://github.com/niklasvh/html2canvas/issues/1878

## how to send PDF file to server

https://github.com/eKoopmans/html2pdf.js/issues/272

https://github.com/eKoopmans/html2pdf.js/issues/271

```ts
// let opt = {
//   filename: `${"generate"}.pdf`,
//   image: { type: "jpeg", quality: 0.6 },
//   html2canvas: {
//     useCORS: true,
//     backgroundColor: null,
//     scrollX: 0,
//     scrollY: 0,
//   },
//   pagebreak: { before: ".break-page", after: ["#after1", "#after2"] },
//   jsPDF: { orientation: "portrait", compressPDF: true }, // unit: 'pt', format: 'a4',
// };

// html2pdf()
//   .set(opt)
//   .from(element)
//   .toPdf()
//   .get("pdf")
//   .then(async function (pdfObj: any) {
//     const perBlob = pdfObj.output("blob");

//     // console.log(perBlob);

//     const { token, key } = await getUploadToken();
//     const formData = new FormData();
//     formData.append("key", key);
//     formData.append("token", token);
//     formData.append("file", perBlob, opt.filename);
//     uploadFile(formData);
//   });
// html2pdf().set(opt).from(element).save();

window.scrollTo(0, 0);
html2canvas(element as any, {
  useCORS: true,
  scale: 1,
}).then(async (canvas) => {
  window.scrollTo(
    0,
    document.body.scrollHeight || document.documentElement.scrollHeight
  );
  setLoading(true);
  try {
    const { token, key } = await getUploadToken();
    uploadToQiniu(
      key,
      token,
      canvas.toDataURL(undefined, 0.6).replace(/^data:image\/\w+;base64,/, "")
    )
      .then((res) => {
        setLoading(false);
        Toast.success(
          "提交成功,3秒回后将自动关闭页面,如果不自动关闭,请手动关闭",
          3,
          () => {
            if (typeof ReactNativeWebView !== undefined) {
              const str = JSON.stringify({
                type: "APP/WebViewClose",
                params: {},
              });
              ReactNativeWebView.postMessage(str);
            }
          }
        );
      })
      .catch((e) => {
        setLoading(false);
      });
  } catch (e) {
    Toast.fail(e.enmsg);
    setLoading(false);
  }
});
```
