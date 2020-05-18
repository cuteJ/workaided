<template>
  <div></div>
</template>

<script>
  let $ = window.go.GraphObject.make

  export default {
    name: 'LinkNode',
    props: {
      modelData: {
        type: [Object],
        required: true
      }
    },
    data() {
      return {
        diagram: null
      }
    },
    mounted() {
      let self = this
      let myDiagram = $(window.go.Diagram, this.$el, {
        'grid.visible': false,
        // 'grid.gridCellSize': new go.Size(10, 10),
        // initialContentAlignment: window.go.Spot.Center,
        initialAutoScale: go.Diagram.Uniform,
        contentAlignment: go.Spot.Center,
        // 鼠标滑动进行缩放
        'toolManager.mouseWheelBehavior': go.ToolManager.WheelZoom,
        // 双击背景创建新节点
        // 'clickCreatingTool.archetypeNodeData': self.initNewNode(),
        'undoManager.isEnabled': false,
        scrollMargin: new go.Margin(100, 200, 100, 100),
        ModelChanged: function (e) {
          self.$emit('model-changed', e)
        },
        ChangedSelection: function (e) {
          self.$emit('changed-selection', e)
        }
      });

      myDiagram.scrollMode = go.Diagram.InfiniteScroll

      // 节点模版
      myDiagram.nodeTemplate = $(
        window.go.Node,
        'Auto',
        new go.Binding('location', 'location', go.Point.parse).makeTwoWay(
          go.Point.stringify
        ),
        $(
          go.Shape,
          'RoundedRectangle',
          {
            parameter1: 20, // the corner has a large radius
            stroke: null,
            portId: '', // this Shape is the Node's port, not the whole Node
            fromLinkable: false,
            fromLinkableSelfNode: false,
            fromLinkableDuplicates: false,
            toLinkable: false,
            toLinkableSelfNode: false,
            toLinkableDuplicates: false,
            cursor: 'pointer'
          },
          new go.Binding('fill', 'type', self.attrFill)
        ),
        $(
          go.TextBlock,
          {
            font: 'bold 11pt helvetica, bold arial, sans-serif',
            editable: false // editing the text automatically updates the model data
          },
          new go.Binding('text').makeTwoWay()
        )
      )

      // 选中样式
      myDiagram.nodeTemplate.selectionAdornmentTemplate = $(
        go.Adornment,
        'Spot',
        $(
          go.Panel,
          'Auto',
          $(go.Shape, {fill: null, stroke: 'blue', strokeWidth: 2}),
          $(go.Placeholder)
        ),
        $(
          'Button',
          {
            alignment: go.Spot.TopRight,
            click: self.addNodeAndLink
          },
          $(go.Shape, 'PlusLine', {width: 6, height: 6})
        )
      );

      // 连接模版
      myDiagram.linkTemplate = $(
        go.Link, // the whole link panel
        {
          // routing: go.Link.Orthogonal,
          // corner: 0,
          // curve: go.Link.JumpGap,
          curve: go.Link.Bezier,
          adjusting: go.Link.Stretch,
          reshapable: false,
          resegmentable: false,
          relinkableFrom: false,
          relinkableTo: false,
          toShortLength: 3
        },
        // new go.Binding('points').makeTwoWay(),
        new go.Binding('curviness'),
        $(
          go.Shape, // the link shape
          {strokeWidth: 1.5}
        ),
        $(
          go.Shape, // the arrowhead
          {toArrow: 'standard', stroke: null}
        ),
        $(
          go.Panel,
          'Auto',
          $(
            go.Shape, // the label background, which becomes transparent around the edges
            {
              fill: $(go.Brush, 'Radial', {
                0: 'rgb(240, 240, 240)',
                0.3: 'rgb(240, 240, 240)',
                1: 'rgba(240, 240, 240, 0)'
              }),
              stroke: null
            }
          ),
          $(
            go.TextBlock,
            '规则', // the label text
            {
              textAlign: 'center',
              font: '9pt helvetica, arial, sans-serif',
              margin: 4,
              editable: false // enable in-place editing
            },
            // editing the text automatically updates the model data
            new go.Binding('text').makeTwoWay()
          )
        )
      );

      myDiagram.linkTemplate.selectionChanged = function (e) {
        self.$emit('changed-link', e)
      };

      // 鼠标右键操作按钮
      myDiagram.nodeTemplate.contextMenu = $(
        go.Adornment,
        'Vertical',
        $('ContextMenuButton', $(go.TextBlock, '查看'), {
          click: function (e, obj) {
            // self.removeNodeAndLink(e, obj)
          }
        }),
      );
      this.diagram = myDiagram;
      this.updateModel(this.modelData)
    },
    watch: {
      modelData: function (val) {
        this.updateModel(val)
      }
    },
    computed: {},
    methods: {
      model() {
        return this.diagram.model
      },
      updateModel(val) {
        if (val instanceof window.go.Model) {
          this.diagram.model = val
        } else {
          let m = new window.go.GraphLinksModel()
          if (val) {
            for (let p in val) {
              m[p] = val[p]
            }
          }
          this.diagram.model = m
        }
      },
      updateDiagramFromData() {
        this.diagram.startTransaction()
        this.diagram.updateAllRelationshipsFromData()
        this.diagram.updateAllTargetBindings()
        this.diagram.commitTransaction('updated')
      },
      attrFill(type) {
        // 节点类型颜色
        const typeColors = [
          ['#00ff00', '#264726'],
          ['#fec900', '#fea200'],
          ['#fec900', '#fea200']
        ]
        let level = type % typeColors.length
        let color = typeColors[type]
        return $(go.Brush, 'Linear', {
          0: color[0],
          1: color[1],
          start: go.Spot.Left,
          end: go.Spot.Right
        })
      },
      initNewNode() {
        return {
          id: uuid.v4().replace(/-/g, ''),
          text: 'new node',
          type: 1
        }
      },
      addNodeAndLink(e, obj) {
        let adornment = obj.part;
        let diagram = e.diagram;
        this.$emit('addToNode', {adornment: adornment, diagram: diagram})
      },
      removeNodeAndLink(e, obj) {
        let node = obj.part.adornedPart
        this.$emit('removeNodeAndLink', node)
      }
    }
  }
</script>

<style>
</style>
