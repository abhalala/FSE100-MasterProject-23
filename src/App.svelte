<script>
  import P5 from "p5-svelte";

  import Viewport from "svelte-viewport-info";

  let value = 0;
  const sketch = (p5) => {
    p5.setup = () => {
      p5.createCanvas(Viewport.Width - 100, Viewport.Height - 100);
    };

    p5.draw = () => {
      p5.fill(value);
      p5.rect(25, 25, 50, 50);
      p5.describe("50-by-50 black rect turns white with touch event.");
    };

    function touchStarted() {
      if (value === 0) {
        value = 255;
      } else {
        value = 0;
      }
    }
  };
</script>

<div class="border-white border-2 p-3 rounded-xl">
  <div>
    <P5 {sketch} />
  </div>
</div>
