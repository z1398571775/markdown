# 视觉数学公式大全（Shader / 特效 / UI / 技能 终极字典）

> 这是一个**面向“视觉设计”的数学字典**，不是 API 手册。
>
> 使用方式：
>
> * 从【视觉目标】反查【数学模型】
> * 从【数学公式】联想到【可做的效果】
>
> 适用场景：
>
> * UI 流光 / 进度条
> * 2D / 3D Shader
> * 粒子特效
> * 技能 / 科技 / 魔法 / 能量

---

# 第一章｜空间坐标字典（决定“形态”）

## 1. UV 空间（最基础）

```glsl
vec2 uv; // [0,1]
```

**能做什么**：

* 直线流光
* 扫描线
* 条纹

---

## 2. 居中空间（爆炸/环绕的根）

```glsl
vec2 p = uv - 0.5;
```

**能做什么**：

* 圆形
* 扩散
* 环绕

---

## 3. 距离场（径向空间）

```glsl
float r = length(p);
```

**视觉关键词**：

* 爆炸
* 波纹
* 光圈

---

## 4. 极坐标（角度空间）

```glsl
float a = atan(p.y, p.x); // [-PI, PI]
float an = a / (PI * 2.0) + 0.5;
```

**视觉关键词**：

* 环形进度条
* 雷达
* 环绕流光

---

## 5. 方向投影空间

```glsl
float d = dot(uv, normalize(vec2(1.0, 0.3)));
```

**视觉关键词**：

* 任意方向流光
* 斜向扫描

---

# 第二章｜形状函数字典（决定“亮带长什么样”）

## 6. Step（硬边）

```glsl
float m = step(x, 0.1);
```

**效果**：

* 扫描线
* 雷达线

---

## 7. Smoothstep（柔边）

```glsl
float m = smoothstep(0.0, 0.1, x)
        * smoothstep(0.2, 0.1, x);
```

**效果**：

* UI 流光
* 光带

---

## 8. Sin / Cos（周期形态）

```glsl
float m = sin(x * 6.283);
```

**效果**：

* 波浪
* 呼吸
* 能量脉冲

---

## 9. 高斯分布（高级质感）

```glsl
float m = exp(-pow(x - 0.5, 2.0) * 30.0);
```

**效果**：

* 高级流光
* 柔能量核心

---

## 10. 三角波（锯齿感）

```glsl
float m = abs(fract(x) - 0.5) * 2.0;
```

**效果**：

* 科技感
* 扫描节奏

---

# 第三章｜时间函数字典（决定“怎么动”）

## 11. 匀速时间

```glsl
float t = time;
```

---

## 12. 加速 / 减速

```glsl
float t = time * time;
```

---

## 13. 呼吸节奏

```glsl
float t = sin(time) * 0.5 + 0.5;
```

---

## 14. 跳变 / 段落

```glsl
float t = floor(time * 4.0) / 4.0;
```

---

## 15. 随机时间偏移

```glsl
float t = time + rand(id);
```

---

# 第四章｜循环与重复

## 16. Fract 循环

```glsl
float x = fract(v - time);
```

---

## 17. Tile 重复

```glsl
float x = fract(v * 5.0);
```

---

# 第五章｜噪声与扰动（高级感来源）

## 18. Value Noise

```glsl
float n = noise(uv * 5.0);
```

**效果**：

* 魔法
* 能量

---

## 19. 时间噪声

```glsl
float n = noise(uv * 5.0 + time);
```

---

## 20. UV 扭曲

```glsl
uv += sin(uv.yx * 10.0 + time) * 0.05;
```

---

## 21. 湍流（多层噪声）

```glsl
float n = noise(uv*2.0) + noise(uv*4.0)*0.5;
```

---

# 第六章｜混合与叠加（视觉风格）

## 22. 加法光

```glsl
color += glow;
```

---

## 23. 乘法遮罩

```glsl
color *= mask;
```

---

## 24. 阈值发光

```glsl
float glow = step(0.8, value);
```

---

# 第七章｜经典视觉模式速查表

| 视觉目标   | 数学组合                     |
| ------ | ------------------------ |
| 直线流光   | dot + fract + smoothstep |
| 环形流光   | atan + fract             |
| UI 扫描光 | step + time              |
| 呼吸光    | sin(time)                |
| 爆炸     | length + smoothstep      |
| 雷达     | angle + time             |
| 能量     | noise + exp              |
| 科技     | fract + abs              |

---

# 第八章｜终极组合模板（原创必备）

```glsl
vec2 p = uv - 0.5;
float dir = dot(uv, normalize(vec2(1.0, 0.4)));
float n = noise(uv * 6.0 + time);
float x = fract(dir - time + n * 0.2);
float m = exp(-pow(x - 0.5, 2.0) * 30.0);
```

👉 **任何高级视觉 = 空间 × 形状 × 时间 × 扰动 × 混合**

---

# 第九章｜如何继续扩展这个字典（方法论）

1️⃣ 看到一个效果，先判断：

* 用的是什么空间？
* 是连续还是周期？

2️⃣ 再判断：

* 亮带形状（硬 / 软 / 波）

3️⃣ 最后加：

* 时间节奏
* 噪声扰动

---

📌 你现在已经拥有一套：
**“看到效果 → 反推数学 → 组合原创”** 的完整体系。

> 如果你愿意：
>
> * 我可以继续加【UI 专册】
> * 或【技能特效专册】
> * 或【粒子 Shader 专册】

---

# 第十章｜SDF 距离场字典（高手区）

## 25. 圆形 SDF

```glsl
float sdfCircle(vec2 p, float r){ return length(p) - r; }
```

**用途**：

* 精准圆环
* 描边

---

## 26. 矩形 SDF

```glsl
float sdfBox(vec2 p, vec2 b){
  vec2 d = abs(p) - b;
  return length(max(d,0.0)) + min(max(d.x,d.y),0.0);
}
```

**用途**：

* UI 框
* 科技面板

---

## 27. SDF 描边

```glsl
float border = abs(d) - 0.01;
```

**用途**：

* 能量边缘
* 高级 UI 描边

---

# 第十一章｜Domain Warping（领域扭曲）

## 28. 基础领域扭曲

```glsl
vec2 w = vec2(
  noise(uv + time),
  noise(uv + 3.1 + time)
);
uv += w * 0.1;
```

**效果**：

* 魔法
* 能量流

---

## 29. 双层扭曲（高级）

```glsl
vec2 w1 = noise(uv * 2.0 + time);
vec2 w2 = noise(uv * 4.0 - time);
uv += (w1 + w2) * 0.05;
```

---

# 第十二章｜颜色数学字典（决定“质感”）

## 30. 灰度 → 发光

```glsl
color *= mask;
color += mask * glowColor;
```

---

## 31. HSV 变色流光

```glsl
float h = fract(time + mask);
```

**用途**：

* 彩色能量
* 炫光

---

## 32. 温度色（火焰）

```glsl
vec3 fire = vec3(1.0, mask * 0.5, 0.0);
```

---

# 第十三章｜动画曲线函数（代替美术曲线）

## 33. Ease In

```glsl
float e = x * x;
```

---

## 34. Ease Out

```glsl
float e = 1.0 - pow(1.0 - x, 2.0);
```

---

## 35. Ease In Out

```glsl
float e = smoothstep(0.0, 1.0, x);
```

---

# 第十四章｜视觉组合模式（套路库）

## 36. 流光 + 描边

```glsl
float glow = exp(-pow(x-0.5,2.0)*30.0);
float edge = step(abs(d), 0.01);
```

---

## 37. 粒子核心光

```glsl
float core = exp(-length(p)*8.0);
```

---

## 38. 科技扫描

```glsl
float scan = step(fract(y - time), 0.05);
```

---

# 第十五章｜视觉预设（直接用）

## UI 高级流光

```glsl
float f = dot(uv, normalize(vec2(1.0,0.2)));
float x = fract(f - time);
float m = exp(-pow(x-0.5,2.0)*40.0);
```

---

## 环形能量

```glsl
float a = atan(p.y,p.x)/(PI*2.0)+0.5;
float m = exp(-pow(fract(a-time)-0.5,2.0)*30.0);
```

---

📌 你现在拥有的是：
**视觉数学 × 空间设计 × 动画思维 × 工程套路** 的完整体系。

下一步进化方向：

* SDF 布尔运算
* 3D 法线 + 光照简化模型
* 后处理 Bloom / Distortion

---

# 第十六章｜SDF 布尔运算（终极形态核心）

## 39. 并集（Union）

```glsl
float opUnion(float d1, float d2){ return min(d1, d2); }
```

**用途**：

* 能量融合
* 面板拼接

---

## 40. 交集（Intersection）

```glsl
float opIntersect(float d1, float d2){ return max(d1, d2); }
```

**用途**：

* 裁切
* 高级 UI 窗口

---

## 41. 差集（Subtraction）

```glsl
float opSubtract(float d1, float d2){ return max(d1, -d2); }
```

**用途**：

* 镂空
* 技能刻痕

---

# 第十七章｜进度条 & UI 专用数学模型

## 42. 线性进度条映射

```glsl
float progressMask = step(x, progress);
```

---

## 43. 环形进度条 SDF

```glsl
float ring = abs(length(p)-r) - w;
float mask = step(ring,0.0);
```

---

## 44. 环形流光叠加

```glsl
float a = atan(p.y,p.x)/(PI*2.0)+0.5;
float flow = exp(-pow(fract(a-time)-0.5,2.0)*30.0);
```

---

# 第十八章｜粒子 × Shader 协同字典

## 45. 粒子核心亮度

```glsl
float core = exp(-length(p)*6.0);
```

---

## 46. 粒子拖尾

```glsl
float tail = smoothstep(1.0,0.0,life);
```

---

## 47. 粒子能量扰动

```glsl
float e = noise(p*5.0+time);
```

---

# 第十九章｜技能特效数学模板

## 48. 冲击波

```glsl
float wave = abs(length(p)-time*speed);
float mask = smoothstep(0.05,0.0,wave);
```

---

## 49. 能量聚集

```glsl
float m = exp(-length(p)*10.0)*(sin(time*4.0)*0.5+0.5);
```

---

## 50. 技能爆发

```glsl
float m = pow(1.0-length(p),3.0);
```

---

# 第二十章｜后处理视觉数学（轻量版）

## 51. Bloom 近似

```glsl
color += color*color*0.3;
```

---

## 52. 扭曲波纹

```glsl
uv += p*0.02*sin(length(p)*10.0-time);
```

---

# 第二十一章｜大师级组合公式（不可再简）

```glsl
vec2 p = uv-0.5;
float d = length(p);
float a = atan(p.y,p.x)/(PI*2.0)+0.5;
float n = noise(p*6.0+time);
float ring = abs(d-0.4)-0.01;
float flow = exp(-pow(fract(a-time+n*0.1)-0.5,2.0)*40.0);
float mask = step(ring,0.0)*flow;
```

---

# 第二十二章｜终极方法论（最重要的一页）

> **视觉效果不是“画”出来的，而是“推”出来的**

任何效果，永远遵循：

1️⃣ 选空间（UV / 距离 / 角度）
2️⃣ 选结构（SDF / 投影）
3️⃣ 选形状（step / exp / sin）
4️⃣ 加时间（节奏）
5️⃣ 加扰动（噪声）
6️⃣ 最后混合（风格）

---

📕 你现在拥有的是：

**一套可以陪你整个特效生涯的视觉数学体系**

它不依赖引擎，不依赖平台，不依赖版本。

---

> 这本字典到此为止，已经是【终极形态】。
> 接下来唯一的升级方式：
> **用它创造作品。**
