# ■ 論文
- 論文タイトル："Deep Residual Learning for Image Recognition"
- 論文リンク：https://arxiv.org/abs/1512.03385
- 論文投稿日付：2015/12/10
- 著者（組織）：(Microsoft Research)
- categories：

# ■ 概要（何をしたか？）

## Abstract

- Deeper neural networks are more difficult to train.
    - より深いネットワークは、学習することがより困難になる。

- We present a residual learning framework to ease the training of networks that are substantially deeper than those used previously. 
    - 我々は、実質的に [substantially]、以前のネットワークより深いような、ネットワークの学習を簡単にする残差 [residual] での学習フレームワークを提供する。

- We explicitly reformulate the layers as learning residual functions with reference to the layer inputs, instead of learning unreferenced functions.
    - 我々は、参照されていない関数を学習する代わりに、
    - 層の入力を参照して [with reference to]、残差関数を学習するように [as]、層を明示的に [explicitly] 再公式化する [reformulate]。

- We provide comprehensive empirical evidence showing that these residual networks are easier to optimize, and can gain accuracy from considerably increased depth.
    - 我々は、これらの残差ネットワークが、最適化することがより簡単であり、
    - 大幅に [considerably] 増加した層の深さから、正確率を得ることが出来る
    - ということを示している包括的で [comprehensive] 実験的な [empirical] 証拠を提供する。

- On the ImageNet dataset we evaluate residual nets with a depth of up to 152 layers — 8 × deeper than VGG nets [41] but still having lower complexity.
    - ImageNet データセットにおいて、我々は、 152 層までの [of up to] 深さで、残差ネットワークを評価する。
    - これは、VGG ネットワークよりも 8 倍深いが、それでもまだ [but still] 複雑さはより低い。

- An ensemble of these residual nets achieves 3.57% error on the ImageNet test set.
    - これらの残差ネットワークのアセンブルは、ImageNet テストデータセットにおいて、エラー率 3.57% を達成する。

- This result won the 1st place on the ILSVRC 2015 classification task.
    - この結果は、ILSVRC 2015 classification task において、第１位 [the 1st place] で勝利した。

- We also present analysis on CIFAR-10 with 100 and 1000 layers.
    - 我々はまた、CIFAR-10 において、100 個の層と 1000 個の層での分析を提示する。

---

- The depth of representations is of central importance for many    visual recognition tasks.
    - 表現の深さは、多くの視覚的な認識タスクにおいて、中心的に重要である。

- Solely due to our extremely deep representations, we obtain a 28% relative improvement on the COCO object detection dataset.
    - 我々の極めて深い表現のみにより [Solely due to]、COCO 物体検出データセットにおいて、28% の相対的な改善が得られる。

- Deep residual nets are foundations of our submissions to ILSVRC & COCO 2015 competitions1, where we also won the 1st places on the tasks of ImageNet detection, ImageNet localization, COCO detection, and COCO segmentation.
    - 深い残差ネットワークは、ImageNet detection, ImageNet localization, COCO detection, and COCO segmentation タスクにおいて、我々が第１位で勝利したILSVRC & COCO 2015 competitions1 への、我々の提案（提出） [submissions] の基礎である。

# ■ イントロダクション（何をしたいか？）

## 1. Introduction

- Deep convolutional neural networks [22, 21] have led to a series of breakthroughs for image classification [21,50, 40].
    - 深い畳み込みネットワークは、画像分類タスクに対して、一連のブレークスルーをもたらした。

- Deep networks naturally integrate low/mid/highlevel features [50] and classifiers in an end-to-end multilayer fashion, and the “levels” of features can be enriched by the number of stacked layers (depth).
    - <font color="Pink">深いネットワークは、低レベル / 中レベル / 高レベル の特徴と分類器を、end-to-end な複数の様式 [fashion] で、自然に統合 [integrate] する。</font>
    - <font color="Pink">そして、特徴のレベルは、積み重ねている層の数によって、豊富になることが出来る。</font>

- Recent evidence [41, 44] reveals that network depth is of crucial importance, and the leading results [41, 44, 13, 16] on the challenging ImageNet dataset [36] all exploit “very deep” [41] models, with a depth of sixteen [41] to thirty [16].
    - 最近の証拠は、ネットワークの深さは極めて重要であるということを明らかにしている。
    - そして、challenging ImageNet dataset において、主要な [leading] 結果は、全て、16 から 30 層の深さを持つとても深いモデルを利用している[exploit]。

- Many other nontrivial visual recognition tasks [8, 12, 7, 32, 27] have also greatly benefited from very deep models.
    - 多くの他の重要ではない [nontrivial] 視覚的認識タスクは、とても深いモデルから、大きな恩恵を受けている。

---

- Driven by the significance of depth, a question arises: Is learning better networks as easy as stacking more layers?
    - 深さの重要性 [significance] によって動かされ、１つの疑問が浮かび上がる。
    - よりよいネットワークを学習することは、より多くの層を積み上げることと同じくらい簡単であるのか？

- An obstacle to answering this question was the notorious problem of vanishing/exploding gradients [1, 9], which hamper convergence from the beginning.
    - この質問への回答することへの障害 [obstacle] は、はじめから収束を妨げる [hamper] ような、悪名高い [notorious] 勾配損失 / 爆発問題である。

- This problem, however, has been largely addressed by normalized initialization [23, 9, 37, 13] and intermediate normalization layers [16], which enable networks with tens of layers to start converging for stochastic gradient descent (SGD) with backpropagation [22].
    - この問題はしかしながら、正規化された初期化と中間層の正規化層によって、大部分が対処（解決）されており [addressed]、
    - このことは、10 個の層を持つネットワークが、誤差逆伝播法で、確率的勾配法（SGD）に対して収束し始めることを可能にする。

---

- When deeper networks are able to start converging, a degradation problem has been exposed: 
    - より深いネットワークが、収束を開始することが出来るとき、低下 [degradation] 問題は露出（暴露）される [exposed]。

- with the network depth increasing, accuracy gets saturated (which might be unsurprising) and then degrades rapidly.
    - **ネットワークの深さの増加と共に、正解率は飽和する。（このことは驚くことではないだろうが）**
    - **そして次に、急速に低下する。**

- Unexpectedly, such degradation is not caused by overfitting, and adding more layers to a suitably deep model leads to higher training error, as reported in [11, 42] and thoroughly verified by our experiments.
    - **意外なことに [Unexpectedly], このような低下は、オーバーフィッティングによって引き起こされる訳ではない。**
    - **そして、[11, 42] や我々の実験で徹底的に [thoroughly] 検証されたように、**
    - **深いモデルの安定化のために、より多くの層を追加することは、より高い学習エラー（＝学習データでの誤識別率）を導く。**

- Fig. 1 shows a typical example.
    - 図１は、典型的な例を示している。

---

![image](https://user-images.githubusercontent.com/25688193/58221192-9efedc80-7d4c-11e9-8917-1b7f0989fa15.png)

- > Figure 1. Training error (left) and test error (right) on CIFAR-10 with 20-layer and 56-layer “plain” networks.
    - 20個の層と56個の層を持つ ”単純な” [plain] ネットワークでの、CIFAR-10 においての、学習エラー（左図）とテストエラー（右図）

- > The deeper network has higher training error, and thus test error.
    - > より深いネットワークは、より高い学習エラーとテストエラーになっている。

- > Similar phenomena on ImageNet is presented in Fig. 4.
    - > ImageNet でのよく似た現象が、図４に示される。

---

- The degradation (of training accuracy) indicates that not all systems are similarly easy to optimize.
    - 学習正解率の低下は、全てのシステムが、同様にして容易に最適化されるわけではないということを、示している。

- Let us consider a shallower architecture and its deeper counterpart that adds more layers onto it.
    - より浅いアーキテクチャと、それにより多くの層を追加するような、対応するもの（＝該当物） [counterpart] （＝ここではアーキテクチャ）を考えよう。

- There exists a solution by construction to the deeper model: the added layers are identity mapping, and the other layers are copied from the learned shallower model.
    - <font color="Pink">より深いモデルへの "construction" による、解決法が存在する。</font>
    - <font color="Pink">即ち、追加された層は、恒等写像であり、</font>
    - <font color="Pink">その他の層は、学習されたより浅いモデルからコピーされている。</font>

- The existence of this constructed solution indicates that a deeper model should produce no higher training error than its shallower counterpart.
    - <font color="Pink">この "construction" での解決法の存在は、より深いモデルは、より浅いモデルよりも、高い学習エラーを生成すべきではないことを示している。</font>

- But experiments show that our current solvers on hand are unable to find solutions that are comparably good or better than the constructed solution (or unable to do so in feasible time).
    - <font color="Pink">しかし、実験は、我々の現在の手元の解決法は、"construction" での解決法よりも、比較してよりよい解決法を見つけることが出来ないよいうことを示している。</font>

---

- In this paper, we address the degradation problem by introducing a deep residual learning framework.
    - この論文では、我々は、深い残差ネットワークのフレームワークを紹介することによって、この低下問題を取り組む [address]。

- Instead of hoping each few stacked layers directly fit a desired underlying mapping, we explicitly let these layers fit a residual mapping.
    - いくつかの積み重ねてられた各層を、望ましい基礎となる [underlying] 写像に、直接的に適合することを望む代わりに、
    - 我々は、明示的に [explicitly]、これらの層を、残差写像に適合する。

- Formally, denoting the desired underlying mapping as H(x), we let the stacked nonlinear layers fit another mapping of F(x) := H(x) - x.
    - 形式的には [Formally]、望ましい基礎となる写像は、H(x) で示し、
    - 積み重ねてられた非線形の層は、F(x) := H(x) - x の他の写像で適合する。

- The original mapping is recast into F(x)+x.
    - オリジナルの写像は、F(x) + x に作り直される [recast]。

- We hypothesize that it is easier to optimize the residual mapping than to optimize the original, unreferenced mapping.
    - **我々は、オリジナルの参照されていない写像を最適化することより、残差写像を最適化することが容易であると仮定する。**

- To the extreme, if an identity mapping were optimal, it would be easier to push the residual to zero than to fit an identity mapping by a stack of nonlinear layers.
    - 極端には、恒等写像が最適であれば、非線形層の積み重ねによって、恒等写像を適合することよりも、残差をゼロにプッシュすることのほうが簡単である。

---

- The formulation of F(x)+x can be realized by feedforward neural networks with “shortcut connections” (Fig. 2).
    - F(x)+x の定式化は、“shortcut connections” を持つ、順伝搬型ニューラルネットワークで実現される [be realized] ことが出来る。（図２）


- Shortcut connections [2, 34, 49] are those skipping one or more layers.
    - Shortcut connections は、１つまたはそれ以上の層をスキップする。

- In our case, the shortcut connections simply perform identity mapping, and their outputs are added to the outputs of the stacked layers (Fig. 2).
    - 我々のケースにおいては、shortcut connections は、恒等写像を実施し、それらの出力は、積み重ねられた層の出力に追加される。（図２）

- Identity shortcut connections add neither extra parameter nor computational complexity.
    - 恒等な shortcut connections は、追加のパラメーターや計算上の複雑さのどちらも追加しない。

- The entire network can still be trained end-to-end by SGD with backpropagation, and can be easily implemented using common libraries (e.g., Caffe [19]) without modifying the solvers.
    - ネットワークの全体は、誤差逆伝播法で、SGD によって、end-to-end に学習可能であり、
    - ソルバーを修正することなしに、共通のライブラリを用いて、容易に実装される。

---

![image](https://user-images.githubusercontent.com/25688193/58225548-76cba980-7d5d-11e9-9710-009b03fab897.png)

- > Figure 2. Residual learning: a building block.

---

- We present comprehensive experiments on ImageNet [36] to show the degradation problem and evaluate our method.
    - 低下問題を見せたり、我々の手法を評価するために、ImageNet において、包括的な [comprehensive] 実験を提示する。

- We show that:

- 1 ) Our extremely deep residual nets are easy to optimize, but the counterpart “plain” nets (that simply stack layers) exhibit higher training error when the depth increases;
    - 1 ) 我々の極端な深い残差ネットワークは、最適化することが容易である。
    - しかし、相当する "簡単な" ネットワーク（単純な層の積み重ね）は、層の深さが増加するとき、より高い学習エラーを展示する。

- 2 ) Our deep residual nets can easily enjoy accuracy gains from greatly increased depth, producing results substantially better than previous networks.
    - 2 ) 我々の深い残差ネットワークは、劇的に増加する深さから、正解率の向上 [gains] を簡単に得ることができ、
    - 以前のネットワークよりも、よい結果を代わりに生成する。

---

- Similar phenomena are also shown on the CIFAR-10 set [20], suggesting that the optimization difficulties and the effects of our method are not just akin to a particular dataset.
    - よく似た現象が、CIFAR-10 で見られ、
    - 最適化の困難さと、我々の手法の効果が、特定のデータセットへの同種 [akin] となっていないことを示している。

- We present successfully trained models on this dataset with over 100 layers, and explore models with over 1000 layers.
    - 我々は、このデータセットにおいて、100 層を超える層で、成功した学習済みモデルを提示し、
    - 1000 層以上で、モデルを調査する。

---

- On the ImageNet classification dataset [36], we obtain excellent results by extremely deep residual nets.
    - ImageNet 分類データセットにおいては、我々は、非常に深い残差ネットワークによって、素晴らしい結果を手に入れる。

- Our 152-layer residual net is the deepest network ever presented on ImageNet, while still having lower complexity than VGG nets [41].
    - 我々の 152 層の残差ネットワークは、ImageNet において、以前のものの中で最も深いネットワークである。
    - 一方で、VGG ネットワークよりも低い複雑さである。

- Our ensemble has 3.57% top-5 error on the ImageNet test set, and won the 1st place in the ILSVRC 2015 classification competition. 
    - 我々のアンサンブルでは、ImageNet テストデータセットにおいて、上位５番目の 3.57% エラー率であった。
    - そして、ILSVRC 2015 classification competition で、第１位で勝利した。

- The extremely deep representations also have excellent generalization performance on other recognition tasks, and lead us to further win the 1st places on: ImageNet detection, ImageNet localization, COCO detection, and COCO segmentation in ILSVRC & COCO 2015 competitions.
    - 非常に深い表現はまた、他の認識タスクにおいても、素晴らしい一般的なパフォーマンスを持つ。
    - 第１でのさらなる勝利に我々を導く。即ち、ImageNet detection, ImageNet localization, COCO detection, and COCO segmentation in ILSVRC & COCO 2015 competitions

- This strong evidence shows that the residual learning principle is generic, and we expect that it is applicable in other vision and non-vision problems.
    - この強い証拠は、残差学習が、原理的に一般性のあるものであり、
    - 他のコンピュータビジョンタスクにおいて、適用可能であることを予期している。

# ■ 結論

## x. 論文の項目名 (Conclusion)


# ■ 何をしたか？詳細

## 3. Deep Residual Learning

### 3.1. Residual Learning

- Let us consider H(x) as an underlying mapping to be
fit by a few stacked layers (not necessarily the entire net),
with x denoting the inputs to the first of these layers. If one
hypothesizes that multiple nonlinear layers can asymptotically
approximate complicated functions2, then it is equivalent
to hypothesize that they can asymptotically approximate
the residual functions, i.e., H(x) 􀀀 x (assuming that
the input and output are of the same dimensions). So
rather than expect stacked layers to approximate H(x), we
explicitly let these layers approximate a residual function
F(x) := H(x) 􀀀 x. The original function thus becomes
F(x)+x. Although both forms should be able to asymptotically
approximate the desired functions (as hypothesized),
the ease of learning might be different.

---

- xxx



# ■ 実験結果（主張の証明）・議論（手法の良し悪し）・メソッド（実験方法）

## x. 論文の項目名


# ■ 関連研究（他の手法との違い）

## x. 論文の項目名（Related Work）

