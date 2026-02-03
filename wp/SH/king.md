1.logic.js->result\=\=win
```js
fetch('check.php', {
  method: 'POST',
  headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
  body: 'result=win'
})
.then(r => r.text())
.then(t => console.log('%c FLAG HERE: ' + t, 'background: #222; color: #bada55; font-size: 20px'));
```


2.
```js

// 获取 Vue 根实例 
var game = document.getElementById('app').__vue__;

// 修改攻击力为 1 亿
game.damage = 100000000;
// 修改攻击速度（数值越小越快，如果是减法逻辑的话；或者直接改 canAttack）
game.speed = 100; 
// 直接瞬移到第 10 关
game.stage = 10;
game.enemiesDefeated = 10;
// 强制触发 Boss 战逻辑
game.boss = true;
```

3.