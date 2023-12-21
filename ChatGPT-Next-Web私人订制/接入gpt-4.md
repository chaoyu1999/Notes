# 1、添加代码

```js
let chatPath;
// Check if model contains 'gpt-4'
if (requestPayload.model.includes('gpt-4')) {
// If it contains 'gpt-4', set chatPath to an empty string
chatPath = "https://dongsiqie-gptnb.hf.space/api/openai/v1/chat/completions";
} else {
// If it doesn't contain 'gpt-4', set chatPath to OpenaiPath.ChatPath
chatPath = this.path(OpenaiPath.ChatPath);
}
// const chatPath = this.path(OpenaiPath.ChatPath);
console.log("[chatPath]",chatPath)
```

2、代码添加位置

![image-20231122161713279](https://raw.githubusercontent.com/onlyfabin/PB_1/main/markdown/202311221617318.png)

`app\client\platforms\openai.ts`