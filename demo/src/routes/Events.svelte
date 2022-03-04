<script>
    import { SvelteGantt, SvelteGanttDependencies, SvelteGanttExternal, SvelteGanttTable, MomentSvelteGanttDateAdapter } from 'svelte-gantt';
    import { onMount, getContext } from 'svelte';
    import { time } from '../utils';
    import moment from 'moment';
    import GanttOptions from '../components/GanttOptions.svelte';

    const currentStart = new Date(new Date().setDate(new Date().getDate()-27));
    const currentEnd = new Date(new Date().setDate(new Date().getDate()+400));

    export const data = {
        rows: [{
            id: 1,
            label: "AccountingsX"
        }, {
            id: 2,
            label: "Business DevelopmentS"
        }, {
            id: 3,
            label: "Ida Flewan"
        }, {
            id: 4,
            label: "Lauréna Shrigley"
        }, {
            id: 5,
            label: "Ange Kembry"
        }, {
            id: 6,
            label: "Business DevelopmentS"
        }, {
            id: 7,
            label: "Ida Flewan"
        }, {
            id: 8,
            label: "Lauréna Shrigley"
        }, {
            id: 9,
            label: "Ange Kembry"
        }, {
            id: 10,
            label: "Business DevelopmentS"
        }, {
            id: 11,
            label: "Ida Flewan"
        }, {
            id: 12,
            label: "Lauréna Shrigley"
        }, {
            id: 13,
            label: "Ange Kembry"
        }, {
            id: 14,
            label: "Business DevelopmentS"
        }, {
            id: 15,
            label: "Ida Flewan"
        }, {
            id: 16,
            label: "Lauréna Shrigley"
        }, {
            id: 17,
            label: "Ange Kembry"
        }, {
            id: 18,
            label: "Business DevelopmentS"
        }, {
            id: 19,
            label: "Ida Flewan"
        }, {
            id: 20,
            label: "Lauréna Shrigley"
        }, {
            id: 21,
            label: "Ange Kembry"
        }, {
            id: 22,
            label: "Business DevelopmentS"
        }, {
            id: 23,
            label: "Ida Flewan"
        }, {
            id: 24,
            label: "Lauréna Shrigley"
        }, {
            id: 25,
            label: "Ange Kembry"
        }, {
            id: 26,
            label: "Business DevelopmentS"
        }, {
            id: 27,
            label: "Ida Flewan"
        }, {
            id: 28,
            label: "Lauréna Shrigley"
        }, {
            id: 29,
            label: "Ange Kembry"
        }, {
            id: 30,
            label: "Business DevelopmentS"
        }, {
            id: 31,
            label: "Ida Flewan"
        }, {
            id: 32,
            label: "Lauréna Shrigley"
        }, {
            id: 33,
            label: "Ange Kembrxy"
        }, {
            id: 34,
            label: "Ange Kembrxy"
        }, {
            id: 35,
            label: "Business DevelopmentS"
        }, {
            id: 36,
            label: "Ida Flewan"
        }, {
            id: 37,
            label: "Lauréna Shrigley"
        }, {
            id: 38,
            label: "Ange Kembry"
        }, {
            id: 39,
            label: "Business DevelopmentS"
        }, {
            id: 40,
            label: "Ida Flewan"
        }, {
            id: 41,
            label: "Lauréna Shrigley"
        }, {
            id: 42,
            label: "Ange Kembry"
        }, {
            id: 43,
            label: "Business DevelopmentS"
        }, {
            id: 44,
            label: "Ida Flewan"
        }, {
            id: 45,
            label: "Lauréna Shrigley"
        }, {
            id: 46,
            label: "Ange Kembry"
        }, {
            id: 47,
            label: "Business DevelopmentS"
        }],
        tasks: [{ id: 1,resourceId: 1,label: 'PET-CT', from: new Date(new Date().setDate(new Date().getDate()-5)), to: new Date(new Date().setDate(new Date().getDate()+10)), classes: 'orange' }],
        dependencies: []
    }

    let options = {
        fitWidth: false,
        columnUnit: 'day',
        dateAdapter: new MomentSvelteGanttDateAdapter(moment),
        rows: data.rows,
        tasks: data.tasks,
        dependencies: data.dependencies,
        timeRanges: [],
        columnOffset: 1,
        magnetUnit: 'day',
        magnetOffset: 1,
        rowHeight: 20,
        rowPadding: 5,
        headers: [{ unit: 'month', format: 'M', sticky: true }, { unit: 'week', format: 'W', sticky: true }, { unit: 'day', format: 'D', sticky: true }], //headers: [{ unit: 'month', format: 'M Y', sticky: true }, { unit: 'week', format: 'w', sticky: true }],
        minWidth: 8000,
        from: currentStart,
        to: currentEnd,
        tableHeaders: [{ title: 'Label', property: 'label', width: 140, type: 'tree' }],
        tableWidth: 200,
        ganttTableModules: [SvelteGanttTable],
        ganttBodyModules: [SvelteGanttDependencies],
        taskElementHook: (node, task) => {
            let popup;
            function onHover() {
                console.log('[task] hover', task);
                popup = createPopup(task, node);
            }

            function onLeave() {
                console.log('[task] hover', task);
                if(popup) {
                    popup.remove();
                }
            }

            node.addEventListener('mouseenter', onHover);
            node.addEventListener('mouseleave', onLeave);

            return {
                destroy() {
                    console.log('[task] destroy');
                    node.removeEventListener('mouseenter', onHover);
                    node.removeEventListener('mouseleave', onLeave);
                }
            }
        },
        // taskContent: (task) => `${task.label} ${task.from.format('HH:mm')}`
    }

    let gantt;
    onMount(() => {
        window.gantt = gantt = new SvelteGantt({ target: document.getElementById('example-gantt-events'), props: options });

        gantt.api.tasks.on.move((task) => console.log('Listener: task move', task));
        //gantt.api.tasks.on.switchRow((task, row, previousRow) => console.log('Listener: task switched row', task));
        gantt.api.tasks.on.select((task) => console.log('Listener: task selected', task));
        //gantt.api.tasks.on.moveEnd((task) => console.log('Listener: task move end', task));
        gantt.api.tasks.on.change(([data]) => console.log('Listener: task change', data));
        gantt.api.tasks.on.changed((task) => console.log('Listener: task changed', task));
        gantt.api.tasks.on.dblclicked((task) => alert('Listener: task double clicked', task));
    });

    function createPopup(task, node) {
        const rect = node.getBoundingClientRect();
        const div = document.createElement('div');
        div.className = 'sg-popup';
        div.innerHTML = `
            <div class="sg-popup-title">${task.label}</div>
            <div class="sg-popup-item">
                <div class="sg-popup-item-label">From:</div>
                <div class="sg-popup-item-value">${new Date(task.from).toLocaleTimeString()}</div>
            </div>
            <div class="sg-popup-item">
                <div class="sg-popup-item-label">To:</div>
                <div class="sg-popup-item-value">${new Date(task.to).toLocaleTimeString()}</div>
            </div>
        `;
        div.style.position = 'absolute';
        div.style.top = `${rect.bottom}px`;
        div.style.left = `${rect.left + rect.width / 2}px`;
        document.body.appendChild(div);
        return div;
    }

    function onChangeOptions(event) {
        const opts = event.detail;
        Object.assign(options, opts);
        gantt.$set(options);
    }
</script>

<style>
    #example-gantt-events {
        flex-grow: 1;
        overflow: auto;
    }

    .container {
        display: flex;
        overflow: auto;
        flex: 1;
    }

    #example-gantt-events :global(.sg-hover) {
        background-color: #00000008;
    }

    #example-gantt-events :global(.sg-hover .sg-table-body-cell) {
        background-color: #00000008;
    }
</style>

<div class="container">
    <div id="example-gantt-events"></div>
    <GanttOptions options={options} on:change={onChangeOptions}/>
</div>