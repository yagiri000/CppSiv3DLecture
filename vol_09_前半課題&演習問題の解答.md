#C++Siv3D講座資料

## 前半課題
これまでやったことを用いて、添付資料のサンプルゲームと似たようなものを作れ。今回は練習として、GameManagerクラスが他のクラスを持つ形にすること。  

## ヒント
解答例は以下のようなクラス構成にした。  

GameManagerクラス(他のクラスを所持し、管理する）  
  |- Playerクラス  
  |- PlayerBulletManagerクラス(PlayerBulletクラスのvectorを所持)  
  |- EnemyManagerクラス(Enemyクラスのvectorを所持)  
  |- EnemyBulletManagerクラス(EnemyBulletクラスのvectorを所持)  
  |- EffectManagerクラス(Effectクラスのvectorを所持)  
  |- ScoreManagerクラス(スコアを管理するクラス)   
