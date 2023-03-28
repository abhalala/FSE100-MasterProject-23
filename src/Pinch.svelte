<script>
  import P5 from "p5-svelte";

  export let touchDist = 0;
  export let percentComplete = 0;
  const sketch = (p5) => {
    p5.setup = () => {
      p5.createCanvas(p5.windowWidth, p5.windowHeight - 20);
    };

    p5.windowResized = () => {
      p5.resizeCanvas(p5.windowWidth, p5.windowHeight - 20);
    };

    p5.draw = () => {
      p5.background("black");

      if (p5.touches.length >= 2) {
        touchDist = p5.dist(
          p5.touches[0].x,
          p5.touches[0].y,
          p5.touches[1].x,
          p5.touches[1].y
        );

        percentComplete = touchDist / p5.height;

        p5.fill("black");
        if (touchDist <= 150) {
          p5.fill("lightgreen");
        }
        p5.strokeWeight(1);
        p5.stroke("white");
        p5.circle(p5.touches[0].x, p5.touches[0].y, 150);
        p5.circle(p5.touches[1].x, p5.touches[1].y, 150);
      }
    };
  };
</script>

<div class="border-black bg-gray-500 text-white touch-none">
  <div>
    <P5 {sketch} />
  </div>
</div>
