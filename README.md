#Digit REcognizer - BPNN vs CNN

MNISTの手書き数字認識タスクにおいて、BPNNとCNNの精度を比較したプロジェクト


特徴量抽出方法の違いによる精度への影響について調べる

##実験結果

kaggleスコア
model   score

BPNN    0.97603

CNN     0.98817

**留意点
全結合層,Dropoutは一致しているが、パラメータ数は異なる


(パラメータ数自体が、特徴量抽出の一部である為)

##考察
精度差の要因
　BPNNは784次元空間に平坦化した画素値をそのまま入力とする為、空間的な位置情報や局所的なパターンの情報が失われる。一方、CNNは畳込み層により、エッジ、曲線などの局所特徴を空間構造を保ったまま抽出できる。この特徴量抽出の手法の差がスコアの差の要因と考えられる。
MNISTによる影響
　BPNNでも0.976と高いスコアを示した。これは今回用いたMNISTが比較的単純なタスクである為であり、より複雑なデータセットでは差はさらに拡大すると考えられる。


##BPNNの構成

層     　　サイズ     活性化関数
入力層     784
隠れ層1    512       ReLU
隠れ層2    256       ReLU
出力層     10        Softmax

##CNNの構成

層             サイズ　　　　活性化関数　  
入力層          28*28*1
Conv2D         32フィルタ    ReLU
MaxPooling2D   2*2 
Conv2D         64フィルタ    ReLU
MaxPooling2D   2*2 
Flatten
__全結合層__
隠れ層1    512       ReLU
隠れ層2    256       ReLU
出力層     10        Softmax

##その他の条件
・optim
　　Adam
・learning_rate
　　0.001
・loss
　　sparse_categorical_crossentropy
・metrics
　　accuracy
・epochs
　　10
・batch_size
　　128
・validation_split
　　0.1



##工夫した点
・BPNNとCNNの特徴量抽出の違いを純粋に比較するため、全結合層の構成（隠れ層1: 512, 隠れ層2: 256, Dropout(0.2)）を両モデルで統一
・Dropoutを各隠れ層の直後に挿入し、過学習抑制

##ディレクトリ構成
