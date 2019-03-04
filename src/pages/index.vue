<template>
    <article class="deep-demo">
        <el-data-table v-bind="tableConfig"
                       dataPath="datas"
                       totalPath="total">

            <el-table-column label="操作" v-bind="{ width: '260px' }">
                <template slot-scope="scope">
                    <el-button  size="small"
                               @click="onDefaultEdit(scope.row)">
                        编辑
                    </el-button>
                    <el-button  type="info" size="small"
                               @click="onDefaultView(scope.row)">
                        查看
                    </el-button>
                    <el-button  type="danger" size="small"
                               @click="onDefaultDelete(scope.row)">
                        下架
                    </el-button>
                </template>
            </el-table-column>
        </el-data-table>
    </article>
</template>

<script>
    import Axios from 'axios'

    export default {
        data() {
            return {
                // 组件名称 分类 版本 开发语言 最后更新时间 状态 操作
                tableConfig: {
                    url: '/deep-demo',
                    hasOperation: false,
                    tableAttrs: {
                        'cell-class-name': ({row, column, rowIndex, columnIndex}) => {
                            if (column.className === 'demo-cell-state')
                                return row.state == 1 ? 'state-online' : 'state-offline'
                        }
                    },
                    columns: [
                        {type: 'selection'},
                        {
                            prop: 'compName',
                            label: '组件名称'
                        }, {
                            prop: 'category',
                            label: '分类'
                        }, {
                            prop: 'version',
                            label: '版本'
                        }, {
                            prop: 'devLang',
                            label: '开发语言'
                        }, {
                            prop: 'lastUpdateDateTime',
                            label: '最后更新时间',
                            formatter: (row, column, cellValue, index) => {
                                var dateTime = new Date(cellValue);
                                return `${dateTime.getFullYear()}-${dateTime.getMonth() + 1}-${dateTime.getDate()}`
                            },
                        }, {
                            prop: 'state',
                            label: '状态',
                            formatter: (row, column, cellValue, index) => {
                                // column.className = row.state == 1 ? 'state-online' : 'state-offline'
                                return cellValue == 1 ? '上架' : '下架'
                            },
                            'class-name': 'demo-cell-state'
                        }
                    ],
                    searchForm: [
                        {
                            $type: 'input',
                            $id: 'compName',
                            label: '组件名称',
                            $el: {
                                placeholder: '请输入组件名称'
                            }
                        }, {
                            $type: 'select',
                            $id: 'category',
                            label: '分类',
                            $el: {
                                placeholder: '请选择分类'
                            },
                            $clearable: true,
                            $options: [{
                                label: 'vue',
                                value: '0'
                            }, {
                                label: 'typescript',
                                value: '1'
                            }]
                        }, {
                            $type: 'select',
                            $id: 'state',
                            label: '状态',
                            $el: {
                                placeholder: '请选择状态'
                            },
                            $clearable: true,
                            $options: [{
                                label: '上线',
                                value: '1'
                            }, {
                                label: '下线',
                                value: '0'
                            }]
                        }
                    ],
                    onDelete: (selected) => {
                        return Axios.delete(
                            '/deep-demo',
                            {
                                data: selected.map(v => v.id)
                            }
                        )
                    },
                    form: [
                        {
                            $type: 'input',
                            $id: 'name',
                            label: '用户名',
                            $el: {
                                placeholder: '请输入'
                            },
                            rules: [
                                {
                                    required: true,
                                    message: '请输入用户名',
                                    trigger: 'blur'
                                }
                            ]
                        }
                    ]
                }
            }
        },
        methods: {}
    }
</script>
<style lang="less">
    .deep-demo {
        // 上线下线样式
        .state-online {
            color: #ff4500;
        }

        .state-offline {
            color: #999;
        }
    }
</style>
