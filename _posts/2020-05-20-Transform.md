---
layout: post
title:  "2D 기하학적 변환 (2D geometric transformation)"
date:   2020-05-20 23:20:00 +0900
category: Image Processing
tags: [영상처리, Image Processing]
---

​     

변환(Transform)은 이미지를 다른 영역(domain)으로 바꾸어 표현하는 것을 의미합니다. 이 글은 특별히 2D 기하학적 변환에 대해서 언급합니다.  기하학적 변환의 종류는 다음과 같습니다.

   

- 이동(Translation)
- 크기 조정(Scaling)
- 반사 (Reflection, Flip)
- 회전(Rotation) 
- 전단(밀림;Shear)

​     

대수식과 행렬을 사용하여 정리해보도록 하겠습니다. (기초적인 내용이라 잘 설명되어 있는 유튜브나 블로그가 많기 때문에, 자세한 설명은 생략합니다.)

​     

#### 이동 (Translation)

$$
Translate(a, b): (x, y) \rightarrow (x+a, y+b) \\
\\ \quad 
\\
\begin{bmatrix} x \\ y \end{bmatrix}
+
\begin{bmatrix} a \\ b \end{bmatrix}
=
\begin{bmatrix} x+a \\ y+b \end{bmatrix}
$$

​     

#### 크기 조정 (Scale)

$$
Scale(a, b): (x, y)\rightarrow(ax, by)
\\ \quad
\\
\begin{bmatrix} a && 0 \\ 0 && b \end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
=
\begin{bmatrix} ax \\ by \end{bmatrix}
$$

​     

#### 반사 (Reflection, Flip)

$$
Reflection_{x-axis}: $(x, y)\rightarrow(-x, y) \\
\\ \quad
\\
\begin{bmatrix} -1 && 0 \\ 0 && 1 \end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
=
\begin{bmatrix} -x \\ y \end{bmatrix} \\
$$

$$
Reflection_{y-axis}: $(x, y)\rightarrow(x, -y) 
\\ \quad
\\
\begin{bmatrix} 1 && 0 \\ 0 && -1 \end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix}
=
\begin{bmatrix} x \\ -y \end{bmatrix}
$$

​     

#### 회전 (Rotation)

$$
Rotate(\theta): (x, y) \rightarrow(x cos(\theta)+y sin(\theta), -x sin(\theta)+y cos(\theta)) 
\\ \quad
\\
\begin{bmatrix} cos(\theta) && sin(\theta) \\
-sin(\theta) && cos(\theta) \end{bmatrix}
\begin{bmatrix} x \\ y \end{bmatrix} = 
\begin{bmatrix} x cos(\theta)+y sin(\theta) \\
-x sin(\theta) + y cos(\theta) \end{bmatrix}
$$

​     

#### 전단 (Share)

$$
Share(a, b): (x, y)\rightarrow(x+ay, y+bx)
\\ \quad
\\
\begin{bmatrix} 1 && a \\ b && 1 \end{bmatrix} 
\begin{bmatrix} x \\ y \end{bmatrix} =
\begin{bmatrix} x+ay \\ y+bx\end{bmatrix}
$$



​     