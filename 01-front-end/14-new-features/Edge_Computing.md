# 边缘计算

## 概述

边缘计算将计算能力下沉到靠近用户的 CDN 节点，降低延迟、减轻源站压力。

---

## 边缘函数

```javascript
// Vercel Edge Function
export const config = {
  runtime: 'edge',
};

export default function handler(req) {
  return new Response(JSON.stringify({ message: 'From Edge!' }), {
    headers: { 'Content-Type': 'application/json' },
  });
}
```

---

## 优势

- 降低网络延迟
- 全球化部署
- 减少服务器负载
- 动态内容就近处理

---

*Related: [[14-new-features/00-Index|新技术]], [[07-network/00-Index|网络]]*
