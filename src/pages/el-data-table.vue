<template>
    <article class="deep-demo">
        <el-data-table v-bind="tableConfig" ref="table"
                       dataPath="datas"
                       totalPath="total">

            <el-table-column label="操作" v-bind="{ width: '260px' }">
                <template slot-scope="scope">
                    <el-button  size="small" type="primary"
                                @click="$refs.table.onDefaultEdit(scope.row)">
                        编辑
                    </el-button>
                    <el-button  type="default" size="small"
                                @click="$refs.table.onDefaultView(scope.row)">
                        查看
                    </el-button>
                    <el-button  type="default" size="small"
                                @click="onSetOnlineOrOff(scope.row)">
                        {{scope.row.state == 0 ? "上架" : "下架"}}
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
                            $id: 'compName',
                            label: '组件名称',
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
                        }, {
                            $type: 'select',
                            $id: 'category',
                            label: '分类',
                            $el: {
                                placeholder: '请选择分类'
                            },
                            rules: [
                                {
                                    required: true,
                                    message: '请选择分类',
                                    trigger: 'blur'
                                }
                            ],
                            $options: [{
                                label: 'vue',
                                value: '0'
                            }, {
                                label: 'typescript',
                                value: '1'
                            }]
                        },  {
                            $type: 'input',
                            $id: 'version',
                            label: '版本',
                            $el: {
                                placeholder: '请输入版本'
                            },
                            rules: [
                                {
                                    required: true,
                                    message: '请输入版本',
                                    trigger: 'blur'
                                }
                            ]
                        }, {
                            $type: 'input',
                            $id: 'devLang',
                            label: '开发语言',
                            $el: {
                                placeholder: '请输入开发语言'
                            },
                            rules: [
                                {
                                    required: true,
                                    message: '请输入开发语言',
                                    trigger: 'blur'
                                }
                            ]
                        }, {
                            $type: 'date-picker',
                            $id: 'lastUpdateDateTime',
                            label: '最后更新时间',
                            $el: {
                                placeholder: '请选择最后更新时间',
                                valueFormat: 'timestamp'
                            },
                            rules: [
                                {
                                    required: true,
                                    message: '请选择最后更新时间',
                                    trigger: 'blur'
                                }
                            ]
                        }, {
                            $type: 'select',
                            $id: 'state',
                            label: '状态',
                            $el: {
                                placeholder: '请选择状态'
                            },
                            rules: [
                                {
                                    required: true,
                                    message: '请选择状态',
                                    trigger: 'blur'
                                }
                            ],
                            $options: [{
                                label: '上架',
                                value: 1
                            }, {
                                label: '下架',
                                value: 0
                            }]
                        }
                    ]
                }
            }
        },
        methods: {
            // 设置上架或下架
            onSetOnlineOrOff(row) {
                row.state = ++row.state %2;
            }
        }
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
