<script>
    import P5 from "p5-svelte";
    let touchInitiated = false;
    let lines = [
        { currentAngle: 0, relativeAngle: null, change: 0, init: 0 },
        { currentAngle: 0, relativeAngle: null, init: 0 },
    ];
    let avgCoords = { x: 0, y: 0 };
    let absoluteChange = 0;
    let absoluteValue = 0;
    let incrementer = 20;
    let won = false;
    let check = 5;

    let numberOfTurns = 0,
        lastNoOfTurns = 0;

    const sketch = (p5) => {
        p5.setup = () => {
            p5.createCanvas(p5.windowWidth, p5.windowHeight - 40);
        };

        p5.windowResized = () => {
            p5.resizeCanvas(p5.windowWidth, p5.windowHeight - 40);
        };

        p5.draw = () => {
            p5.background("black");

            if (p5.touches.length == 5) {
                lines[0].currentAngle =
                    (Math.atan2(
                        p5.touches[0].y - p5.touches[2].y,
                        p5.touches[0].x - p5.touches[2].x
                    ) *
                        180) /
                    Math.PI;

                lines[1].currentAngle =
                    (Math.atan2(
                        p5.touches[1].y - p5.touches[3].y,
                        p5.touches[1].x - p5.touches[3].x
                    ) *
                        180) /
                    Math.PI;

                if (touchInitiated) {
                    lines.forEach((l) => {
                        l.relativeAngle = l.init - l.currentAngle;
                    });

                    absoluteChange = (lines[0].change + lines[1].change) / 2;
                    avgCoords.x =
                        (p5.touches[0].x +
                            p5.touches[1].x +
                            p5.touches[2].x +
                            p5.touches[3].x) /
                        4;
                    avgCoords.y =
                        (p5.touches[0].y +
                            p5.touches[1].y +
                            p5.touches[2].y +
                            p5.touches[3].y) /
                        4;

                    absoluteValue =
                        (Math.abs(
                            Math.abs(lines[0].relativeAngle) +
                                Math.abs(lines[1].relativeAngle)
                        ) /
                            360) *
                        200;

                    if (absoluteValue >= incrementer) {
                        numberOfTurns =
                            Math.floor(absoluteValue / 20) +
                            (check > 5 && !won ? lastNoOfTurns : 0);
                        incrementer += 20;
                    }
                    p5.fill(0, 200 + numberOfTurns * 10, 0);
                    p5.circle(avgCoords.x, avgCoords.y, 100 * numberOfTurns);
                }
            }
        };

        p5.touchStarted = () => {
            if (check <= 5 || won) {
                numberOfTurns = 0;
            }
            incrementer = 20;
            if (p5.touches.length == 5) {
                lines[0].init =
                    (Math.atan2(
                        p5.touches[0].y - p5.touches[2].y,
                        p5.touches[0].x - p5.touches[2].x
                    ) *
                        180) /
                    Math.PI;

                lines[1].init =
                    (Math.atan2(
                        p5.touches[1].y - p5.touches[3].y,
                        p5.touches[1].x - p5.touches[3].x
                    ) *
                        180) /
                    Math.PI;

                touchInitiated = true;
            }
        };

        p5.touchEnded = () => {
            touchInitiated = false;
            if (numberOfTurns >= check) won = true;
            if (check > 5 && !won) {
                lastNoOfTurns = numberOfTurns;
            }
        };

        p5.touchDragged = () => {};
    };
</script>

<div class="grid justify-center">
    <h1 class="p-2 uppercase text-5xl font-bold font-mono">
        Score: {numberOfTurns} / {check}
    </h1>
    {#if won}
        <h1 class="p-3 uppercase text-4xl font-bold font-sans text-green-400">
            Congratulations!
        </h1>
        <button
            on:click={() => {
                won = false;
                check += 5;
            }}
            class="rounded-md p-5 uppercase font-bold text-xl bg-green-500"
            >Next Level</button
        >
    {/if}
</div>
<div class="bg-gray-500 text-white touch-none h-full w-screen">
    <div>
        <P5 {sketch} />
    </div>
</div>
