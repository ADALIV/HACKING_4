# HACKING_4
## <https://dreamhack.io/wargame/challenges/96/>
<br/>
<br/>
Web basic hacking question. Click on the pumpkin 10000 times and catch the flag! If you go into developer tools and look at the basic HTML structure of the element, it is as follows.
<br/>
<br/>

```
var pumpkin = [ 124, 112, 59, 73, 167, 100, 105, 75, 59, 23, 16, 181, 165, 104, 43, 49, 118, 71, 112, 169, 43, 53 ];
var counter = 0;
var pie = 1;

function make() {
  if (0 < counter && counter <= 1000) {
    $('#jack-nose').css('opacity', (counter) + '%');
  }
  else if (1000 < counter && counter <= 3000) {
    $('#jack-left').css('opacity', (counter - 1000) / 2 + '%');
  }
  else if (3000 < counter && counter <= 5000) {
    $('#jack-right').css('opacity', (counter - 3000) / 2 + '%');
  }
  else if (5000 < counter && counter <= 10000) {
    $('#jack-mouth').css('opacity', (counter - 5000) / 5 + '%');
  }

  if (10000 < counter) {
    $('#jack-target').addClass('tada');
    var ctx = document.querySelector("canvas").getContext("2d"),
    dashLen = 220, dashOffset = dashLen, speed = 20,
    txt = pumpkin.map(x=>String.fromCharCode(x)).join(''), x = 30, i = 0;

    ctx.font = "50px Comic Sans MS, cursive, TSCu_Comic, sans-serif"; 
    ctx.lineWidth = 5; ctx.lineJoin = "round"; ctx.globalAlpha = 2/3;
    ctx.strokeStyle = ctx.fillStyle = "#1f2f90";

    (function loop() {
      ctx.clearRect(x, 0, 60, 150);
      ctx.setLineDash([dashLen - dashOffset, dashOffset - speed]); // create a long dash mask
      dashOffset -= speed;                                         // reduce dash length
      ctx.strokeText(txt[i], x, 90);                               // stroke letter

      if (dashOffset > 0) requestAnimationFrame(loop);             // animate
      else {
        ctx.fillText(txt[i], x, 90);                               // fill final letter
        dashOffset = dashLen;                                      // prep next char
        x += ctx.measureText(txt[i++]).width + ctx.lineWidth * Math.random();
        ctx.setTransform(1, 0, 0, 1, 0, 3 * Math.random());        // random y-delta
        ctx.rotate(Math.random() * 0.005);                         // random rotation
        if (i < txt.length) requestAnimationFrame(loop);
      }
    })();
  }
  else {
    $('#clicks').text(10000 - counter);
  }
}

$(function() {
  $('#jack-target').click(function () {
    counter += 1;
    if (counter <= 10000 && counter % 100 == 0) {
      for (var i = 0; i < pumpkin.length; i++) {
        pumpkin[i] ^= pie;
        pie = ((pie ^ 0xff) + (i * 10)) & 0xff;
      }
    }
    make();
  });
});
```

<br/>
<br/>
In this code, can you guess which code change count of clicks?
<br/>
<br/>

```
$(function() {
  $('#jack-target').click(function () {
    counter += 1;
    if (counter <= 10000 && counter % 100 == 0) {
      for (var i = 0; i < pumpkin.length; i++) {
        pumpkin[i] ^= pie;
        pie = ((pie ^ 0xff) + (i * 10)) & 0xff;
      }
    }
    make();
  });
});
```

<br/>
<br/>
More than 10000 counts, it will execute make() function. But we cannot modify this function or source code directly. It's local file's code! So we must use console to make difference.
<br/>
<br/>

```
counter = 9999
```

<br/>
<br/>
Like above code, you can modify counter variable, and with just one more click, you can get answer. And also this code will work.
<br/>
<br/>

```
for (i=0; i<=10000; i++) {
    $('#jack-target').click()
}
```
