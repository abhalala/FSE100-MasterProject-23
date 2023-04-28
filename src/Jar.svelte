<script>
    import P5 from "p5-svelte";
    let initialSlopeA, initialSlopeB;
    let a = 0,
        b = 0;
    let newA = 0,
        newB = 0;
    let aP = 0,
        bP = 0;
    const sketch = (p5) => {
        p5.setup = () => {
            p5.createCanvas(p5.windowWidth, p5.windowHeight - 40);
        };

        p5.windowResized = () => {
            p5.resizeCanvas(p5.windowWidth, p5.windowHeight - 40);
        };

        p5.draw = () => {
            p5.background("black");
            const p0 = p5.touches[0], p1 = p5.touches[1], p2 = p5.tocuhes[2], p3 = p5.touches[3];
            a = newA;
            b = newB;
            const slopeA = (p0.y - p2.y) / (p0.x - p2.x);
            const slopeB = (p1.y - p3.y) / (p1.x - p3.x);

            if (slopeA > initialSlopeA) {
                newA = slopeA - initialSlopeA;
            } else if (slopeA < initialSlopeA) {
                newA = initialSlopeA - slopeA;
            }

            if (slopeB > initialSlopeB) {
                newB = slopeB - initialSlopeB;
            } else if (slopeB < initialSlopeB) {
                newB = initialSlopeB - slopeB;
            }

            if (a > newA) {
                aP -= 1;
            } else if (a < newA) {
                aP += 1;
            }

            if (b > newB) {
                bP -= 1;
            } else if (b < newB) {
                bP += 1;
            }
        };

        p5.touchStarted = () => {
            initialSlopeA =
                (p5.touches[0].y - p5.touches[2].y) /
                (p5.touches[0].x - p5.touches[2].x);
            initialSlopeB =
                (p5.touches[1].y - p5.touches[3].y) /
                (p5.touches[1].x - p5.touches[3].x);
        };
    };
</script>

<div class="bg-gray-500 text-white touch-none h-full w-screen">
    aP: {aP} | bP: {bP}
    <div>
        <P5 {sketch} />
    </div>
</div>
