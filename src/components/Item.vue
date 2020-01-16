<template>
    <div class="resize-drag" data-x=0 data-y=0 >
        Resize from any edge or corner
    </div>
</template>
<script>
    import interact from "interactjs"

    export default {
        name: "Item",
        mounted() {
            interact('.resize-drag')
                .resizable({
                    // resize from all edges and corners
                    edges: { left: true, right: true, bottom: true, top: true },

                    modifiers: [
                        // keep the edges inside the parent
                        interact.modifiers.restrictEdges({
                            outer: 'parent'
                        }),

                        // minimum size
                        interact.modifiers.restrictSize({
                            min: { width: 100, height: 50 }
                        })
                    ],

                    inertia: true
                })
                .draggable({
                    onmove: this.dragMoveListener,
                    inertia: true,
                    modifiers: [
                        interact.modifiers.restrictRect({
                            restriction: 'parent',
                            endOnly: true
                        })
                    ]
                })
                .on('resizemove', function (event) {
                    const target = event.target;
                    let x = (parseFloat(target.getAttribute('data-x')) || 0);
                    let y = (parseFloat(target.getAttribute('data-y')) || 0);

                    // update the element's style
                    target.style.width = event.rect.width + 'px';
                    target.style.height = event.rect.height + 'px';

                    // translate when resizing from top or left edges
                    x += event.deltaRect.left;
                    y += event.deltaRect.top;

                    target.style.webkitTransform = target.style.transform =
                        'translate(' + x + 'px,' + y + 'px)';

                    target.setAttribute('data-x', x);
                    target.setAttribute('data-y', y);
                    target.textContent = Math.round(event.rect.width) + '\u00D7' + Math.round(event.rect.height)
                })

            window.dragMoveListener = this.dragMoveListener;
        },
        methods: {
            dragMoveListener(event) {
                const target = event.target
                // keep the dragged position in the data-x/data-y attributes
                let x = (parseFloat(target.getAttribute('data-x')) || 0) + event.dx
                let y = (parseFloat(target.getAttribute('data-y')) || 0) + event.dy

                // translate the element
                target.style.webkitTransform =
                    target.style.transform =
                        'translate(' + x + 'px, ' + y + 'px)'

                // update the posiion attributes
                target.setAttribute('data-x', x)
                target.setAttribute('data-y', y)
            }
        }
    }
</script>
<style scoped>
    .resize-drag {
        background-color: #29e;
        color: white;
        font-size: 20px;
        font-family: sans-serif;
        border-radius: 8px;
        padding: 20px;
        touch-action: none;

        width: 120px;

        /* This makes things *much* easier */
        box-sizing: border-box;
    }
</style>