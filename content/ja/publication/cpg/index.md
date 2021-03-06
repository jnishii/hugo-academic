---
title: 多足歩行の制御モデル
tags:
- "Neural Network"
- "motor control"
weight: 10

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["3"]
---

{{<video src="tripod.mp4" controls="true" width="400">}}

多足歩行動物は，様々な路面形状や外乱に対応しつつ，さらに，移動速度に応じて歩幅，脚運動周期，歩容(複数の脚が交互に動く順番)なども変化させて，安定かつ効率の良い歩行を実現しています。
このような歩行の制御メカニズムとして，足部が地面についたら踏ん張るといった反射の連鎖によるという**反射説**，CPG (Central Pattern Generator)と呼ばれる神経系による周期活動によるという**CPG説**などが提唱されています。

ロボティクス的手法や機械学習に基づく手法を使って，反射やCPGによる多足歩行の学習をどのように行えるかを検討してきました。

<!--
### 階層的運動制御モデル

実際の歩行運動は，反射系とCPGの両方で実現されている可能性もあります。
そこで，反射系とCPGがどのように協調すると、望ましい運動パターンを学習・発生できるか, さらに，そのような多重制御のメリットは何かについて検討してきました。
-->

<!--[Related papers](../papers/#cpg)-->
