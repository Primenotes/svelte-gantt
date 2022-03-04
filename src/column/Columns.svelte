<script lang="ts">
    /**
     * Container component for columns rendered as gantt body background
     */
    export let columns = [];

    export let columnStrokeWidth = 1;
    export let columnStrokeColor = '#efefef';

    //alert('columns.length' + columns.length);

    function lineAt(ctx, x) {
        alert(x);
        ctx.beginPath();
        ctx.moveTo(80, 0);
        ctx.lineTo(80, 50);
        ctx.stroke();
    }

    function createBackground(columns) {
        alert(JSON.stringify(columns));
        const canvas = document.createElement('canvas');
        canvas.width = (columns.length -1) * columns[0].width;
        alert('columns width' + (columns.length -1) * columns[0].width);
        canvas.height = 20;

        const ctx = canvas.getContext('2d');
        ctx.shadowColor = "rgba(128,128,128,0.5)";
        ctx.shadowOffsetX = 0;
        ctx.shadowOffsetY = 0;
        ctx.shadowBlur = 1;
        ctx.lineWidth = columns[0].width * 2; //columnStrokeWidth;
        ctx.lineCap = "square";
        ctx.strokeStyle = columnStrokeColor;
        ctx.translate(2, 2);

        ctx.beginPath();
        ctx.moveTo(((columns.length -1) * columns[0].width) - columns[0].width, 0);
        ctx.lineTo(((columns.length -1) * columns[0].width) - columns[0].width, 10);
        ctx.stroke();

        //columns.forEach((column, index) => {
            //if (index === 2) {
            //    lineAt(ctx, column.left);
            //alert(JSON.stringify(column));
            //alert(index);
            //lineAt(ctx, column.left);
            //} 
        //});

        alert(canvas);
        const dataURL = canvas.toDataURL();
        return `url("${dataURL}")`;
    }

    let backgroundImage;
    $: {
        alert(JSON.stringify(columns.slice(0,7)));
        backgroundImage = createBackground(columns.slice(0,8));
        alert(backgroundImage);
    }
</script>

<div class="sg-columns" style="background-image:{backgroundImage};">
	<!---{#each columns as column}
	<column left={column.left} width={column.width} />
	{/each} --->
</div>

<style>
    .sg-columns {
        position: absolute;
        height: 100%;
        width: 100%;
        overflow: hidden;

        background-repeat: repeat;
        background-position-x: -1px;
    }
</style>