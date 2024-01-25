请求 URL:

https://https.api.phind.com/agent/

请求方法:

POST

Content-Type:

application/json;charset=UTF-8

User-Agent:

Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0

Sec-Ch-Ua-Platform:

"Windows"



Phind格式：

```json
{
    "user_input": "你会什么？",
    "message_history": [
        {
            "role": "user",
            "content": "你是？"
        },
        {
            "role": "assistant",
            "content": "我是Phind创建的编程助手，专门帮助编程者解决复杂的编程、技术或信息查询任务。"
        },
        {
            "role": "user",
            "content": "你会什么？"
        }
    ],
    "requested_model": "Phind Model",
    "challenge": 0.5
}
```



openai格式：

```json
{
    "messages": [
        {
            "role": "user",
            "content": "你是？"
        },
        {
            "role": "assistant",
            "content": "我是Phind创建的编程助手，专门帮助编程者解决复杂的编程、技术或信息查询任务。"
        },
        {
            "role": "user",
            "content": "你会什么？"
        }
    ],
    "stream": true,
    "model": "gpt-3.5",
    "temperature": 0.5,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "top_p": 0
}
```

phind回复：

```
data: {"type": "metadata", "model": "phind"}
data: {"id": "chatcmpl-aki2qtuvp4gswft9j0dw", "object": "chat.completion.chunk", "created": 1706076445, "model": "trt-llm-phind-model-serving", "choices": [{"index": 0, "delta": {"role": "assistant", "content": "", "name": "base"}, "finish_reason": null}]}
data: {"id": "chatcmpl-aki2qtuvp4gswft9j0dw", "object": "chat.completion.chunk", "created": 1706076445, "model": "trt-llm-phind-model-serving", "choices": [{"index": 0, "delta": {"content": "\u6211"}, "finish_reason": null}]}
data: {"id": "chatcmpl-aki2qtuvp4gswft9j0dw", "object": "chat.completion.chunk", "created": 1706076445, "model": "trt-llm-phind-model-serving", "choices": [{"index": 0, "delta": {}, "finish_reason": "stop"}]}
```

openai 回复：

```
data: {"id":"chatcmpl-8kREaBDyQTOYbF6fCCdHBDupB88bp","object":"chat.completion.chunk","created":1706078468,"model":"gpt-3.5-turbo-0613","system_fingerprint":null,"choices":[{"index":0,"delta":{"content":"助"},"logprobs":null,"finish_reason":null}]}
data: {"id":"chatcmpl-8kREaBDyQTOYbF6fCCdHBDupB88bp","object":"chat.completion.chunk","created":1706078468,"model":"gpt-3.5-turbo-0613","system_fingerprint":null,"choices":[{"index":0,"delta":{"content":"。"},"logprobs":null,"finish_reason":null}]}
data: {"id":"chatcmpl-8kREaBDyQTOYbF6fCCdHBDupB88bp","object":"chat.completion.chunk","created":1706078468,"model":"gpt-3.5-turbo-0613","system_fingerprint":null,"choices":[{"index":0,"delta":{},"logprobs":null,"finish_reason":"stop"}]}
data: [DONE]
```

cloudflare worker逆向代码：

```

// 将OpenAI请求格式转换为Phind请求格式的函数
function convertFormat(openaiFormat) {
  // 加载JSON数据
  openaiFormat = JSON.parse(openaiFormat);
  let inp = openaiFormat.messages[openaiFormat.messages.length - 1].content;
  openaiFormat.messages[openaiFormat.messages.length - 1].content = "这是我的要求：请不要向我提问，请回答我的问题就算你不懂！下面是我的输入：\n" + inp;
  // 定义Phind格式
  var phindFormat = {
    'user_input': "这是我的要求：请不要向我提问，请回答我的问题就算你不懂！下面是我的输入：\n" + inp,
    'message_history': openaiFormat.messages,
    'requested_model': 'Phind Model',
    'challenge': openaiFormat.temperature
  };
  return phindFormat;
}

function processData(d) { // 将Phind返回格式转成openai格式
  let _data = d.replace("data: ", "");
  try {
    let data_dict = JSON.parse(_data);
    if ('id' in data_dict && 'content' in data_dict['choices'][0]['delta']) {
      let delta_length = data_dict['choices'][0]['delta'].length;
      data_dict['system_fingerprint'] = "fp_aaa20cc2ba";
      data_dict['choices'][0]['logprobs'] = null;
      if (delta_length === 3) {
        delete data_dict['choices'][0]['delta']['name'];
      }

      if ('role' in data_dict['choices'][0]['delta'] && data_dict['choices'][0]['delta']['role'] == "function") {
        delete data_dict['choices'][0]['delta']['name'];
        delete data_dict['choices'][0]['delta']['role'];
        let res = data_dict['choices'][0]['delta']['metadata']['web_results']
        for (let i = 0; i < res.length && i < 3; i++) {
          let c = data_dict['choices'][0]['delta']['content']
          data_dict['choices'][0]['delta']['content'] = `搜索结果${3 - (i+1)}: ${res[i]["url"]} \n` + c
        }
        let c = data_dict['choices'][0]['delta']['content']
        data_dict['choices'][0]['delta']['content'] += '\n'

        delete data_dict['choices'][0]['delta']['metadata'];
      }
      _data = "data: " + JSON.stringify(data_dict) + '\n';

      return _data;

    } else {
      return '\n';
    }
  } catch (error) {
    return '\n';
  }
}

export default {
  async fetch(request, env, ctx) {
    console.log("访问：" + new URL(request.url).pathname);
    if (new URL(request.url).pathname.startsWith("/v1/chat/completions") && request.method === "POST") {
      const incomingData = await request.text();
      const convertedData = convertFormat(incomingData);
      const data = JSON.stringify(convertedData);
      console.log(data);

      const url = "https://https.api.phind.com/agent/";
      const headers = {
        "Content-Type": "application/json;charset=UTF-8",
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0",
      };

      const response = await fetch(url, {
        method: 'POST',
        headers: headers,
        body: data,
      });
      // 创建一个新的可读流
      let decoder = new TextDecoder();
      let encoder = new TextEncoder();
      const stream = new ReadableStream({
        start(controller) {
          const reader = response.body.getReader();
          let buffer = '';

          function push() {
            reader.read().then(({ done, value }) => {
              if (done) {
                if (buffer.length > 0) {
                  console.log(buffer);
                }
                controller.enqueue(encoder.encode("data: [DONE]\n\n"));
                controller.close();
                return;
              }
              // 解析数据
              buffer += decoder.decode(value, { stream: true });
              let newlineIndex;
              while ((newlineIndex = buffer.indexOf('\n')) !== -1) {
                let line = buffer.slice(0, newlineIndex);
                line = processData(line)
                console.log(line)
                buffer = buffer.slice(newlineIndex + 1);
                controller.enqueue(encoder.encode(line));
              }
              push();
            });
          }

          push();
        }
      });
      return new Response(stream, {
        headers: {
          "Content-Type": "text/event-stream",
          "Cache-Control": "no-cache",
          "Connection": "keep-alive",
        }
      });
    } else {
      if (request.method === "OPTIONS") {
        // 处理 OPTIONS 请求
        return new Response(null, {
          headers: {
            "Allow": "POST",
            "Access-Control-Allow-Methods": "POST",
            "Access-Control-Allow-Headers": "Content-Type"
          }
        });
      }
      return new Response("Error!", {
        headers: {
          "Cache-Control": "no-cache",
        }
      });
    }
  },
};
```

