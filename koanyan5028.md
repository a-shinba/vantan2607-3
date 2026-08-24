# WebAudioAPI上での、双一次変換を使用した一次ハイパス・ローパスフィルタの実装
※出典: 自身のFamicomPlayerのコード
```js
const context :AudioContext;
const sampleRate :number=context.sampleRate;

// https://ja.wikipedia.org/wiki/ハイパスフィルタ
function createHighPassFilter(frequency :number){
  // Sample Time
  // 双一次変換のTに対応
  let st=1/sampleRate;
  // Frequency (Analog)
  // 周波数歪みの補正
  let fa=(2/st*Math.tan((2*Math.PI*frequency)*st/2))/(2*Math.PI);
  // Time Constant
  // ハイパスフィルタのτに対応
  let tc=1/(2*Math.PI*fa);

  let b=[2*tc/(st+2*tc),-2*tc/(st+2*tc)];
  let a=[1,(st-2*tc)/(st+2*tc)];
  return context.createIIRFilter(b,a);
}

// https://ja.wikipedia.org/wiki/ローパスフィルタ
function createLowPassFilter(frequency :number){
  // Sample Time
  // 双一次変換のTに対応
  let st=1/sampleRate;
  // Frequency (Analog)
  // 周波数歪みの補正
  let fa=(2/st*Math.tan((2*Math.PI*frequency)*st/2))/(2*Math.PI);
  // Time Constant
  // ローパスフィルタのτに対応
  let tc=1/(2*Math.PI*fa);

  let b=[st/(st+2*tc),st/(st+2*tc)];
  let a=[1,(st-2*tc)/(st+2*tc)];
  return context.createIIRFilter(b,a);
}
```
