<script>
    import { SvelteGantt, SvelteGanttDependencies, SvelteGanttExternal, SvelteGanttTable, MomentSvelteGanttDateAdapter } from 'svelte-gantt';
    import { onMount, getContext } from 'svelte';
    import { time } from '../utils';
    import moment from 'moment';
    import GanttOptions from '../components/GanttOptions.svelte';

    const currentStart = new Date(new Date().setDate(new Date().getDate()-5));
    const currentEnd = new Date(new Date().setDate(new Date().getDate()+400));

    const colors = ['blue', 'green', 'orange','red']
    
    let options2 = getContext('options');

    export const data = {
        rows: [{
            id: 1,
            label: "AccountingS",
        }, {
            id: 2,
            label: "Business Development",
        }, {
            id: 3,
            label: "Ida Flewan"
        }, {
            id: 4,
            label: "Lauréna Shrigley"
        }, {
            id: 5,
            label: "Ange Kembry"
        }],
        tasks: [{
            id: 3,
            resourceId: 1,
            label: "AK 503",
            from: time("13:30"),
            to: time("15:00"),
            classes: "orange"
        }, {
            id: 4,
            resourceId: 1,
            label: "Auditing",
            from: time("9:30"),
            to: time("11:30"),
            classes: "red"
        }, {
            id: 5,
            resourceId: 2,
            label: "Security Clearance",
            from: time("15:15"),
            to: time("16:00"),
            classes: "green"
        }, {
            id: 6,
            resourceId: 2,
            label: "Policy Analysis",
            from: time("14:00"),
            to: time("17:00"),
            classes: "blue"
        }, {
            id: 7,
            resourceId: 2,
            label: "Xbox 360",
            from: time("13:00"),
            to: time("14:00"),
            classes: "blue"
        }, {
            id: 8,
            resourceId: 3,
            label: "GNU/Linux",
            from: time("14:00"),
            to: time("15:30"),
            classes: "orange"
        }, {
            id: 9,
            resourceId: 4,
            label: "Electronic Trading",
            from: time("15:00"),
            to: time("17:00"),
            classes: "green"
        }, {
            id: 10,
            resourceId: 5,
            label: "Alternative Medicine",
            from: time("14:30"),
            to: time("15:30"),
            classes: "orange"
        }],
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
        window.gantt = gantt = new SvelteGantt({ target: document.getElementById('example-gantt'), props: options });
        const external = new SvelteGanttExternal(document.getElementById('new-task'), {
            gantt,
            onsuccess: (row, date, gantt) => {
                console.log(row.model.id, new Date(date).toISOString())
                const id = 5000 + Math.floor(Math.random() * 1000); //Ubound all id's!!
                gantt.updateTask({
                    id,
                    label: `Neues Element_${id}`,
                    from: date,
                    to: date + 3 * 60 * 60 * 1000,
                    classes: colors[(Math.random() * colors.length) | 0],
                    resourceId: row.model.id
                });
            },
            elementContent: () => {
                const element = document.createElement('div');
                element.innerHTML = 'Neues Element';
                element.className = 'sg-external-indicator';
                return element;
            }
        });
    });

    function onChangeOptions(event) {
        const opts = event.detail;
        Object.assign(options, opts);
        gantt.$set(options);
    }
</script>

<style>
    #example-gantt {
        flex-grow: 1;
        overflow: auto;
    }

    .container {
        display: flex;
        overflow: auto;
        flex: 1;
    }

    #new-task {
        position: absolute;
        bottom: 0;
        right: 0;
        z-index: 1;
        background-color: #ee6e73;
        color: white;
        padding: 1rem;
        margin: 0.5rem;
        cursor: grab;
    }
</style>

<div class="container">
    <div id="example-gantt"></div>
    <div id="new-task">Neues Element</div>
    <GanttOptions options={options} on:change={onChangeOptions}/>
</div>