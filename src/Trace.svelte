<script>
  import P5 from "p5-svelte";
  import bird from "./bird.json";
  import rabbit from "./rabbit.json";
  let showNextButton = false;
  let currentLevel = 1;
  let levels = [
    {
      pointsDone: [],
      percentDone: 0,
      dots: bird.map((c) => {
        return {
          x: c[0],
          y: c[1],
          traced: false,
        };
      }),
    },
    {
      pointsDone: [],
      percentDone: 0,
      dots: rabbit.map((c) => {
        return {
          x: c[0],
          y: c[1],
          traced: false,
        };
      }),
    },
  ];

  let aliveLevel = levels[currentLevel - 1];

  const sketch = (p5) => {
    p5.setup = () => {
      p5.createCanvas(p5.windowWidth, p5.windowHeight - 40);
    };
    p5.windowResized = () => {
      p5.resizeCanvas(p5.windowWidth, p5.windowHeight - 40);
    };

    p5.draw = () => {
      aliveLevel = levels[currentLevel - 1];
      p5.background(0);
      p5.strokeWeight(5);
      aliveLevel.dots.forEach((c) => {
        if (c.traced) {
          p5.stroke("green");
        } else {
          p5.stroke("white");
        }

        p5.point(c.x, c.y);
      });

      if (p5.touches.length >= 1) {
        aliveLevel.dots = aliveLevel.dots.map((c) => {
          let d = p5.dist(p5.touches[0].x, p5.touches[0].y, c.x, c.y);
          if (d <= 20) {
            c.traced = true;
          }
          return c;
        });
      }

      if (aliveLevel.percentDone === 100) {
        showNextButton = true;
      }

      aliveLevel.pointsDone = aliveLevel.dots.filter(
        ({ traced }) => traced == true
      );
      aliveLevel.percentDone =
        (aliveLevel.pointsDone.length / aliveLevel.dots.length) * 100;
    };
  };
</script>

<div class="grid justify-center">
  <h1 class="p-2 uppercase text-5xl font-bold font-mono">
    Level {currentLevel}
  </h1>
  <h2 class="p-3 uppercase text-2xl font-bold font-mono">
    Traced: {aliveLevel.percentDone.toPrecision(3)}%
  </h2>
  {#if showNextButton && !(currentLevel == 2)}
    <h1 class="p-3 uppercase text-4xl font-bold font-sans text-green-400">
      Congratulations!
    </h1>
    <button
      on:click={() => {
        showNextButton = false;
        currentLevel += 1;
      }}
      class="rounded-md p-5 uppercase font-bold text-xl bg-green-500"
      >Next Level</button
    >
  {:else if aliveLevel.percentDone === 100}
    <h1 class="p-3 uppercase text-4xl font-bold font-sans text-green-400">
      Congratulations!
    </h1>
    <h1 class="p-3 uppercase text-4xl font-bold font-sans text-green-400">
      You've Won the game.
    </h1>
  {/if}
</div>
<div class="bg-gray-500 text-white touch-none h-full w-screen">
  <div>
    <P5 {sketch} />
  </div>
</div>
