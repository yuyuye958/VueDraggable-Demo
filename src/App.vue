<template>
  <div id="app">
    <main>
      <!--运算符选择-->
      <section class="operator">
        <h3>运算符</h3>

        <div class="parameter">
          <p class="parameter-title">参数：</p>
          <draggable v-model="parameter" :options="{group:{ name:'main', pull:'clone', put:false }}" @start="moveParameter">
            <el-button class="my-btn" v-for="(element, index) in parameter" :key="index">
              {{element.name}}
            </el-button>
          </draggable>
        </div>

        <div class="parameter">
          <p class="parameter-title">逻辑运算符：</p>
          <draggable v-model="logic" :options="{group:{ name:'main', pull:'clone', put:false }}">
            <el-button class="my-btn" v-for="(element, index) in logic" :key="index">
              {{element.name}}
            </el-button>
          </draggable>
        </div>

        <div class="parameter">
          <p class="parameter-title">数字运算符：</p>
          <draggable v-model="number" :options="{group:{ name:'main', pull:'clone', put:false }}">
            <el-button class="my-btn" v-for="(element, index) in number" :key="index">
              {{element.name}}
            </el-button>
          </draggable>
        </div>

      </section>

      <!--条件逻辑关系-->
      <section class="relation">
        <h3>条件逻辑关系</h3>
        <draggable v-model="relation" :options="{group:'main'}" class="editing">
          <div v-for="(element,index) in relation">

            <div class="relation-item" v-if="element.type === 'parameter'">
              <div class="relation-item-inner">
                <button class="close-btn" @click="remove(index)">X</button>
                <el-select v-model="value[index]" placeholder="请选择">
                  <el-option
                    v-for="item in element.options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>
              </div>
            </div>

            <div class="relation-item" v-if="element.type === 'logic'">
              <div class="relation-item-inner">
                <button class="close-btn" @click="remove(index)">X</button>
                <el-button class="my-btn" v-for="item in element.options">{{item}}</el-button>
              </div>
            </div>

            <div class="relation-item" v-if="element.type === 'number'">
              <div class="relation-item-inner">
                <button class="close-btn" @click="remove(index)">X</button>
                <el-button class="my-btn">{{element.name}}</el-button>
              </div>

              <div class="relation-item-inner">
                <button class="close-btn" @click="remove(index)">X</button>
                <el-input class="my-input" placeholder="请输入内容"></el-input>
              </div>
            </div>
          </div>

        </draggable>
      </section>
    </main>
  </div>
</template>

<script>
  import draggable from 'vuedraggable'

  export default {
    name: 'App',
    components: {draggable},
    data() {
      return {
        parameterStatus: false,
        logicStatus: false,
        numberStatus: false,
        value: [''],
        parameter: [
          {
            type: 'parameter',
            name: '模板参数',
            options: [
              {
                value: '选项1',
                label: '伺服轴数'
              }, {
                value: '选项2',
                label: '路径数'
              }, {
                value: '选项3',
                label: '报警数量'
              }, {
                value: '选项4',
                label: '主轴轴数'
              }
            ],
          }
        ],
        logic: [
          {
            type: 'logic',
            name: '括号',
            options: ['(', ')']
          }
        ],
        number: [
          {
            type: 'number',
            name: '等于'
          },
          {
            type: 'number',
            name: '大于'
          },
          {
            type: 'number',
            name: '小于'
          },
        ],
        relation: []
      }
    },
    methods: {
      remove(index) {
        this.relation.splice(index, 1)
      },
      moveParameter(){
        this.$data.$set(value2,false)
      }
    }
  }
</script>

<style lang="scss" scoped>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  #app {
    max-width: 1200px;
    margin: 0 auto;
    main {
      display: flex;
      justify-content: space-between;
      section {
        width: 50%;
        height: 300px;
        margin-right: 10px;
        padding: 15px;
        border: 1px solid red;
        h3 {
          margin-bottom: 0.5em;
        }
        .my-btn {
          padding: 12px 20px;
        }
      }
    }
  }

  .operator {
    .parameter {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
      .parameter-title {
        min-width: 130px;
      }
    }
  }

  .relation {
    div {
      display: inline-block;
    }
    .editing {
      height: 100%;
      width: 100%;
    }
    .my-input {
      width: 180px;
    }
    .relation-item {
      margin-bottom: 0.5em;
      margin-right: 0.5em;
      .relation-item-inner {
        position: relative;
        .close-btn {
          position: absolute;
          top: 0;
          right: 0;
          z-index: 1;
          display: none;
        }
        &:hover {
          .close-btn {
            display: inline-block;
          }
        }
      }
    }

  }
</style>
