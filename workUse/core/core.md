# core

## ~/src/core/utils

### $confirm  确认框

- 示例1
<img src=".\img\Snipaste_2024-07-04_08-46-14.jpg" align="left"/>

```tsx
await $confirm({
        text: `是否删除`,
        type: "warning"
});
```



```tsx
export async function $confirm(params: ILdAlertParams) {
    await _elMessageBox({
        ...params,
        tipType: 'confirm',
        from: `[来自Web]`
    })
};
```

- 示例2
  <img src=".\img\2.jpg" align="left"/>

````tsx
$confirm({
        text: () => (
            <div>
                <p onClick={() => alert('hello p2')}>This is VNode</p>
            </div>
        ),
        type: "warning",
        topTitle: 'topTitle',
        confirmText: 'confirmText',
        cancelText: 'cancelText',
        beforeClose: (done) => {   // 点击确认按钮后执行
            alert('hello');
            done(); // 调用 done 函数关闭弹窗
        },
    	options: {     
    		callback: () => {     // callback关闭或取消后执行
        		alert("closing")
    		}
		}
});
````

- 参数

```tsx
//ILdAlertParams参数
text: string | VNode | (() => VNode),  // 可以为字符串或者vnode
type: TipType,           // type TipType = "info" | "success" | "error" | "warning" | "none";
topTitle?: string,		 // 标题
confirmText?: string,	 // 确认文本
cancelText?: string,	 // 取消文本
beforeClose?: (next: () => void) => void,   // 点击确认按钮后执行，需要手动调用done方法关闭弹窗
options?: ElMessageBoxOptions    // element-plus 原本的api

//text，type为必选参数
```

### $confirmApi

与$confirm类似

显示来自api

```tsx
export async function $alertApi(params: ILdAlertParams) {
    await _elMessageBox({
        ...params,
        tipType: "alert",
        from: `[来自Api]`
    })
}
```



###  $alert 警告框

- 示例1

<img src=".\img\3.jpg" align="left"/>



```tsx
$alert({
    text: 'text',
    type: 'warning'
})
```

- 参数  同confirm

```tsx
text: string | VNode | (() => VNode),  // 可以为字符串或者vnode
type: TipType,           // type TipType = "info" | "success" | "error" | "warning" | "none";
topTitle?: string,		 // 标题
confirmText?: string,	 // 确认文本
cancelText?: string,	 // 取消文本
beforeClose?: (next: () => void) => void,   // 点击确认按钮后执行，需要手动调用done方法关闭弹窗
options?: ElMessageBoxOptions    // element-plus 原本的api
```



### $alertApi

同$alert 

显示来自APi



### $message  顶部提示框

<img src=".\img\5.jpg" align="left"/>

```tsx
$message.error(text, desc?, duration?): 显示错误类型的消息提示。
$message.info(text, desc?, duration?): 显示信息类型的消息提示。
$message.loading(text, desc?, duration?): 显示加载中类型的消息提示。
$message.success(text, desc?, duration?): 显示成功类型的消息提示。
$message.warning(text, desc?, duration?): 显示警告类型的消息提示。 
```

```tsx\
text (必需):
	类型: string
	描述: 消息提示的主要内容,用于显示提示的主要信息。
	示例: "删除成功"、"操作失败"、"请输入有效的邮箱地址" 等。
desc (可选):
	类型: string
	描述: 消息提示的附加描述信息,用于提供更详细的说明或补充信息。
	示例: "请联系管理员查看详细错误信息"、"请检查网络连接并重试" 等。
duration (可选):
	类型: number
	描述: 消息提示显示的持续时间,以毫秒为单位。默认值为 3000,即 3 秒。
	示例: 5000 表示消息提示将显示 5 秒钟,2000 表示消息提示将显示 2 秒钟。
```



###  $messageApi  

同$message,来自Api



###  showLoading  正在加载
<img src=".\img\4.jpg" align="left"/>

```tsx
showLoading()
```



- 参数

```tsx
text: string = "加载中"   // 指定加载中提示的文本内容，默认为加载中
call?:()=>Promise<any>   // 异步函数,在显示加载中提示的同时执行一些异步操作.当 call 函数执行完成后，加载中提示会自动关闭。
```





- 示例1

```tsx
const showLoadingTest = async () => {
    await showLoading("正在处理中", async () => {
        await new Promise((resolve) => {
            setTimeout(() => {
                resolve(null);
            }, 2000);
        });
    });
};

<div>
    <ElButton onClick={showLoadingTest}>
        showLoading1
    </ElButton>
</div>
```



-  示例2 

```tsx
//手动调用closeLoading结束加载动画
import { showLoading , closeLoading} from "~/src/core/utils";

const showLoadingTest = () => {
       showLoading();
    
       setTimeout(() => {
         closeLoading();
       }, 2000);   // 2秒后调用 closeLoading()
}

<ElButton onClick={showLoadingTest}>
     showLoading
</ElButton>
```



## ~/src/core/model/hooks

### useLdTsxDialog  对话框

<img src=".\img\6.jpg" align="left"/>

- 参数

```tsx
//useLdTsxDialog 接受三个参数
export const useLdTsxDialog = (callFunc: () => JSX.Element,
  param: DialogDfConfig['IOptions'] = {}, callBack?: (json: DialogDfConfig["ICallBackParam"]) => any)
  
1.callFunc：一个返回 JSX.Element 的函数，用于渲染对话框的内容。

2.param：对话框的配置选项，默认为空对象 {}。
export interface DfConfigIOptions {
  /** escape 是否关闭弹框 */
  closeEscape?: boolean,
  /** 标题 */
  title?: string,
  /** 宽度 - 传入数字时，宽度为内容宽度，不是总宽 */
  width?: string | number,
  /** 是否默认打开 */
  openShow?: boolean,
  /** 保持弹窗高度位最大高度 */
  keepMaxHeight?: boolean;
  /** 确定按钮的文本内容 */
  confirmButtonText?: string,
  /** 确认按钮快捷键配置 */
  confirmButtonKeyboard?: IButtonKeyboard,
  /** 取消按钮的文本内容 */
  cancelButtonText?: string,
  /** 取消按钮快捷键配置 */
  cancelButtonKeyboard?: IButtonKeyboard,
  /** 是否显示保存按钮 */
  showConfirmButton?: boolean,
  /**是否显示右上角关闭 */
  showCloseButton?: boolean,
  /** 是否显示取消按钮 */
  showCancelButtonText?: boolean,
  /** 内边距 可传四个值 上右下左 */
  padding?: string,
  /** 显示外框 */
  showFrame?: boolean,
  /** 是否开启最大化 */
  maximize?: boolean,
  /** 是否可拖动 - 默认false */
  draggable?: boolean,
  /** 是否为全屏 Dialog */
  fullscreen?: boolean,
  /** 固定高度 - 内部自适应 */
  fixedHeight?: boolean,
  /** 按钮扩展 */
  tsxButton?: any,
  /** 键盘快捷键 */
  keyboardLst?: IKeydownConfing[],
  /** 自定义标题 */
  tsxTitle?: () => JSX.Element,
  /** 自定义关闭左侧 */
  tsxCloseLeft?: () => JSX.Element,
  /** 关闭后自动销毁 */
  autoDestory?: boolean
}

3.callBack：对话框的回调函数，可选。
/** 定义第三个回调函数传参类型  open 默认只触发一次*/
ICallBackParam: {
  type: "save" | "close" | "open" | "pre-close" | "pre-open"
}
```



- 示例

```tsx
const dialog = useLdTsxDialog(() => {
        return (<>
         xxx
        </>)
    }, {
        title: "机器设置",
        width: '90vw'
    }, async (types) => {
        switch (types.type) {
            case "save":
                break;
            default:
                break;
        }
    })


```

- 返回值

`useLdTsxDialog` 函数返回一个对象，其中包含了 `getControl` 和 `getAsynControl` 两个方法

```tsx
const control = await dialog.getAsynControl();  //getAsynControl 是异步获取控制器对象的方法
			  = dialog.getControl();   			//getControl 是同步获取控制器对象的方法
```

```tsx
//control的方法
1.control.open(title?: string, forse?: boolean, config?: IOpenConfig): Promise<void>
	描述:打开对话框的方法。
	参数:title:可选,对话框的标题。
		forse:可选,是否强制打开对话框。
		config:可选,打开对话框的配置选项。
	返回值:Promise<void>,表示异步打开对话框。
2.close(): void
	描述:关闭对话框的方法。
	返回值:void,表示同步关闭对话框。
3.save(): void
	描述:触发保存。
	返回值:void,表示同步触发保存操作。
4.setTitle(title: string): void
	描述:设置标题。
	参数:title:对话框的新标题。
	返回值:void,表示同步设置对话框的标题。
5.focus(): void
	描述:聚焦到确认按钮。
	返回值:void,表示同步将焦点移动到确认按钮上。
6.setConfirmButtonText(str: string): void
	描述:修改确定按钮文字。
	参数:str:新的确定按钮文字。
	返回值:void,表示同步修改确定按钮的文字。
7.setCancelButtonText(str: string): void
	描述:修改取消按钮文字。
	参数:str:新的取消按钮文字。
	返回值:void,表示同步修改取消按钮的文字。
8.setShowConfirmButton(bool: boolean): void
	描述:修改确定按钮是否显示。
	参数:bool:是否显示确定按钮。
	返回值:void,表示同步修改确定按钮的显示状态。
9.setShowCancelButtonText(bool: boolean): void
	描述:修改取消按钮是否显示。
	参数:bool:是否显示取消按钮。
	返回值:void,表示同步修改取消按钮的显示状态。

```



### useLdVxeTable

<img src=".\img\12.jpg" align="left"/>

#### 参数

```tsx
export function useLdVxeTable<T>(param: VxeTableDfConfig<T>["IOptions"],
callBack?: (json: VxeTableDfConfig<T>["ICallBackParam"]) => any): LdVxeTableModel<T>
    
1. param: VxeTableDfConfig<T>["IOptions"]
	表示访问 `VxeTableDfConfig<T>` 接口中的 `IOptions` 属性的类型
	VxeTableDfConfig<T> 是一个类型别名,表示 VXETable 组件的默认配置类型。
	IOptions 是配置选项的接口类型。
 
IOptions: IOptionsMain<T>    
export interface IOptionsMain<T> extends ILdTableDfConfig<T> {
	/** 列 column 的配置数组 */
	column: TableColumn<T>[];
	/** 获取接口数据方法，data不传时执行通过该方法获取数据 */
	getList: (pageConfig?: IVxeTableGetListParams, vxeQuery?: VxeGridPropTypes.ProxyAjaxQueryParams) => Promise<any>;
}   //column 和 getList是必填参数

export interface ILdTableDfConfig<T> {
	/** 列 column 的配置数组 */
	column?: TableColumn<T>[];
	/** 是否默认调用getList方法 默认值 true */
	enableGetList?: true | false,
	/** 获取接口数据方法，data不传时执行通过该方法获取数据 */
	getList?: (pageConfig?: IVxeTableGetListParams, vxeQuery?: VxeGridPropTypes.ProxyAjaxQueryParams) => Promise<any>;
	/** 是否选中第一个 */
	checkedFirst?: boolean;
	/** 表头是否可自由拖拽 */
	sortable?: boolean;
	/** 默认选中最后一个 */
	checkedLast?: boolean;
	/** 固定行高 */
	fixRowHeight?: boolean;
	/** 设置所有内容过长时显示为省略号（如果是固定列建议设置该值，提升渲染速度） */
	showOverflow?: boolean;
	/** 过滤缓存 - 可以保留数据过滤筛选 */
	filterCache?: boolean;
	/** 表格选择模式 single：单选  none：不能选择  checkbox：多选 cell: 选择单元格  */
	selectMode?: "single" | "none" | "checkbox" | "cell",
	/** 树形选择框模式 none：不作任何处理 cell：单选，父子互不干扰  group：成组选中 ,cell-parent: 父选中时选中所有子 */
	treeCheckboxMode?: "none" | "cell" | "group" | "cell-parent",
	/** 是否允许勾选的方法，该方法，的返回值用来决定这一行的 checkbox 是否可以勾选 */
	checkboxDisabledMethod?: (props: { row: any }) => boolean,
	/** 是否允许勾选的方法，该方法，的返回值用来决定这一行的 checkbox 是否显示 */
	checkboxVisibleMethod?: (props: { row: any }) => boolean,
	/** 隐藏复选框全选框 */
	hideCheckBoxHeader?: boolean;
	/** 是否在打开左侧复选框 */
	selection?: boolean;
	/** 是否开启虚拟滚动 默认开启 */
	virtual?: boolean;
	/** 单行添加单行class */
	rowClassFunction?: (row: { rowData: any }) => string,
	/** 指定selection的key，默认会在原数据中添加selection字段 */
	selectionKey?: string;
	/** 可选择框位置 */
	selectionPosition?: "left",
	/** 树形选择时父子选项互不干扰 */
	checkStrictly?: boolean,
	/** 选择框class名 */
	selectClassName?: string,
	/** table的ref对象 */
	tableRef?: Ref<VxeGridInstance | undefined>;
	/** 一页几条数据 */
	pageSize?: number;
	/** 是否开启分页 */
	pageOpen?: boolean;
	/** 单击选中表格在右侧展示纵向信息，模式为single才生效 */
	showDetail?: boolean | IShowDetail;
	/** 是否为树形数据 */
	isTree?: boolean;
	/** 哪些字段生成前端拼音码 */
	pymList?: string[];
	/** 隐藏第一个树column占位 */
	treeHide?: boolean;
	/** 树形数据配置 */
	treeConfig?: VxeTablePropTypes.TreeConfig;
	/** 默认展开的key */
	defaultExpandedRowKeys?: any;
	/** 右键菜单配置 */
	menuConfig?: IMenuOption[];
	/** 显示左下角搜索 */
	showSearch?: boolean;
	/** 是否开启搜索过滤 */
	searchFilter?: boolean;
	/** 搜索时不使用深拷贝 */
	searchNotClone?: boolean;
	/** 不改变原数据状态搜索 */
	searchSourceData?: boolean;
	/** 打开搜索功能 - 外置搜索框时使用 */
	useSearch?: boolean;
	/** 导出 */
	export?: IExport;
	/** 开启单元格搜索 */
	cellSearch?: boolean | {
		/** 是否启用 */
		enabled: boolean
		/** 是否过滤掉搜索以外的数据,只保留搜索出来的数据 */
		filter?: boolean
	},
	/** 打开后端排序 */
	openApiSort?: boolean | {
		/** 是否启用 */
		enabled: boolean
		/** 是否启用多列组合筛选 */
		multiple?: boolean
	},
	/** 响应式data对象 */
	showData?: Ref<any[]>,
	/** 总是打开所有层级 */
	alwaysExpand?: boolean
	/** 选中的行数据 */
	selectRow?: (Ref<UnwrapRef<any | null>>);
	/** 选中的行数据 */
	selectColumnId?: (Ref<string>);
	/** 占位高度 */
	height?: number;
	/** 底部是否有padding */
	footPadding?: boolean;
	/** 宽度 */
	width?: number,
	/** 是否打开左侧复选框 */
	openSelectionCell?: boolean,
	/** 没有滚条 */
	noRollBar?: boolean,
	/** vxe官方配置项【优先级最高】 */
	vxeConfig?: VxeGridProps,
	/** vxe官方触发事件【优先级最高】 */
	vxeEvents?: VxeGridListeners
}
    
2.callBack?: (json: VxeTableDfConfig<T>["ICallBackParam"]) => any
这是一个可选的回调函数,用于处理表格事件和数据变化。
ICallBackParam: { 
		type:
		"selectChange"
		| "dbClick"
		| "selectClick"
		| "RightMenu"
		| "dataMounted"
		| "enter"
		| "cellSelected"
		| "TreeExpandChange"
		| "checkBoxChange"
		| "filterChange"
		| "pageChange"
		,
		data: any,
		columnId?: string,
		event?: any
}
```

#### column参数

```tsx
/** 原数据格式继承vxe-table格式 */
export type TableColumn<T> = VxeTableDefines.ColumnOptions & {
	/** 是否显示排序 */
	sortable?: boolean,
	/** 标题 */
	title?: string,
	/** 列宽 */
	width?: number | string,
	/** 是否为可搜索字段 */
	isSearchKey?: boolean,
	/** 唯一标识 - 后端返回接口的key */
	dataKey?: keyof T,
	/** 字典 */
	dict?: TDictStrType | Dict,
	/** 字典按多个值查找还是单个值查找 （multy：多个  single：单个） */
	dictType?: 'multy' | 'single'
	/** 开启自动筛选 */
	filter?: boolean,
	/** 编辑表格 - 是否必传 */
	required?: boolean,
	/** 编辑表格 - 验证规则 */
	rules?: VxeTableDefines.ValidatorRule[],
	/** 唯一标识 - 【可不传】 */
	id?: number | string,
	/** 重写数据 - 作用在表格加载数据时（状态为loading时） */
	cellRewrite?: (props: ICellRewrite<T>) => (Promise<string> | string),
	/** 单位 */
	unit?: string | IUnit<T>,
	/** 重写单元格渲染函数 */
	cellRenderer?: (props: {
		row: any;
		cellData: any, column: any, rowData: T & { [key: string]: any }, rowIndex: any
	}) => any,
	/** 重写表头渲染函数 */
	headerCellRenderer?: (props?: IHeaderCellRenderer<T>) => any,
	children?: TableColumn<T>[]
}

export interface ColumnOptions extends VxeColumnProps {
    children?: ColumnOptions[]
    slots?: VxeColumnPropTypes.Slots
}
```

#### 返回值

```tsx
export interface LdVxeTableModel<T> extends IRetModel<VxeTableDfConfig<T>> {
    /** 获取到当前表格组件的实例引用,可以使用该实例调用表格提供的各种方法 */
  getTableRef: () => Ref<VxeGridInstance | undefined>,
    /** 可以获取到当前选中行的引用 */
  getRowRef: <A extends T>() => Ref<A | null | undefined>,  
    /** 获取到当前选中列的ID */    
  getColumnIdRef: () => Ref<string>
}

export interface IRetModel<T extends IModule> {
    /** 表格组件本身 */        
  component: any;
    /** 表格的配置选项 */  
  options: T["IOptions"];
    /** 表格的回调函数, */  
  callBack: ((obj: T["ICallBackParam"]) => void) | null;
    /** 返回表格的控制对象,可以通过该对象调用表格提供的各种控制方法 */  
  getControl: () => T["IControl"];
    /** 一个异步函数,返回一个 Promise,resolved 值为表格的控制对象 */  
  getAsynControl: () => Promise<T["IControl"]>;
    /** 一个函数,接收表格的控制对象作为参数,可以在这个函数中监听控制对象的变化。 */  
  controlListener: (control: T["IControl"]) => void;
}
```

#### 示例1

```tsx
import { defineComponent, h } from 'vue'
import { useLdVxeTable } from '~/src/core/model/hooks';
import { DictSelect, LdForm, LdFormItem, Model } from "~/src/core/components";

export default defineComponent({

    setup(props, { slots, expose, emit, attrs }) {
        interface testIN {
            id?: number,
            XM?: string,
            XH?: string,
        }
        const testData = [
            {
                id: 1,
                XM: '张三',
                XH: '2019200'
            },
            {
                id: 2,
                XM: '李四',
                XH: '2019201'
            }
        ]
        const table = useLdVxeTable<testIN>({
            selection: true, 
            async getList() { return testData },
            column: [
                {
                    width: 100,
                    title: '编号',
                    dataKey: 'id',
                },
                {
                    width: 100,
                    title: "姓名",
                    dataKey: "XM"
                },
                {
                    width: 100,
                    title: "学号",
                    dataKey: 'XH'
                },
            ]
        }, (obj: any) => {
            switch (obj.type) {
                case "selectChange":
                    break;
                default:
                    break;
            }
        })
        return () => (
            <div>
                <Model model={table}>
                </Model>
            </div>
        )
    }
})

// 渲染表格 <Model model={table}></Model>
```

#### 渲染表格

```tsx
// 1. 直接用model渲染
<Model model={table}></Model>

// 2. 封装为函数式组件时，先用element return出去，再用h函数渲染
return {
        element: () => {
            return <Model model={table}></Model>
        }
    }

const table1 = useTable1();

{h(table1.element)}
```



#### 表尾添加数据

写在model内

```tsx
<Model model={table}>
	<span>hello</span>
</Model>
```





### useLdVxeTableAsync

- 参数

```tsx
export function useLdVxeTableAsync<T>(fun: (getTable: () => (LdVxeTableModel<T> | null)) => Promise<LdVxeTableModel<T>>)
```



- 示例

```tsx
import { ElButton, ElInput } from 'element-plus';
import { defineComponent, ref } from 'vue';
import { ModelAsync } from '~/src/core/components';
import { useLdVxeTableAsync } from '~/src/core/hooks';
import { useLdVxeTable } from '~/src/core/model/hooks';
import { sleep } from '~/src/core/utils';
import { apiDataToWebColumns } from './renderRule';

export default defineComponent({
    setup() {

        const tableAsync = useLdVxeTableAsync<any>(async (getTable) => {

            const column = apiDataToWebColumns([
                {
                    dataKey: 'data',
                    title: "日期时间",
                    width: 200,
                    sortable: true
                },
                {
                    dataKey: 'ruliang',
                    title: "入量",
                    children: [
                        {
                            title: "入量名称",
                            dataKey: "rlmc",
                            width: 100,
                            renderType: 'input_1'
                        },
                        {
                            title: "量",
                            dataKey: "l",
                            width: 100,
                            renderType: 'input_num'
                        },
                        {
                            title: "单位",
                            dataKey: "dw",
                            width: 100,
                            renderType: 'input_1'
                        }
                    ]
                }
            ], getTable)

            await sleep(1000);

            return useLdVxeTable({
                column,
                vxeConfig: {
                    toolbarConfig: {
                        slots: {
                            buttons: () => {
                                return [<ElButton onClick={async () => {
                                    const newRecord = {};
                                    const retData = await tableAsync.getTable()?.getTableRef().value?.insertAt(newRecord, -1);
                                    await tableAsync.getTable()?.getTableRef().value?.setEditRow(retData?.row);
                                }}>插入一行</ElButton>]
                            }
                        }
                    }
                },
                getList: async () => {
                    return []
                }
            });
        })

        return () => (
            <>
                <ModelAsync model={tableAsync}></ModelAsync>
            </>
        );
    }
});

```

#### 刷新数据

```tsx
table.getControl().getAjaxListData();
```

#### 回调

```typescript
type: "selectChange" | "dbClick" | "selectClick" | "RightMenu" | "dataMounted" | "enter" | "cellSelected" | "TreeExpandChange" | "checkBoxChange" | "filterChange" | "pageChange"
```





## ~/src/core/components

### 组件

#### ButtonSelect 按钮选择

<img src=".\img\13.jpg" align="left" />

```tsx
<ButtonSelect v-model={form.showflag} onChange={() => {
    configStore.resetTableHeight()
    updataForm()
}}>
    <ButtonSelectItem label={'入库'} value={'rk'}></ButtonSelectItem>
    <ButtonSelectItem label={'出库'} value={'ck'}></ButtonSelectItem>
    <ButtonSelectItem label={'入出库汇总'} value={'rckhz'}></ButtonSelectItem>
</ButtonSelect>
```





#### DictSelect 下拉框

<img src=".\img\7.jpg" align="left" />

- 参数

```tsx
dict：字典类型，可以是字典的字符串类型或 Dict 对象。
	type: String as PropType<TDictStrType | Dict>
getList：数据源接口，返回一个 Promise，用于获取下拉选项数据。
	type: Object as PropType<() => Promise<{ label: string, value: string, [key: string]: any }[]>>
data：直接传入的下拉选项数据，类型为对象数组，每个对象包含 label 和 value 属性。
	type: Object as PropType<{ label: string; value: string, [key: string]: any }[]>
showAll：是否默认显示"全部"选项，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
readonly：是否为只读模式，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
label：下拉选择框的标签文本，类型为字符串，默认为空字符串。
	type: String as PropType<string>
width：下拉选择框的宽度，类型为数字，单位为像素。
	type: Number as PropType<number>
size：下拉选择框的尺寸，可选值为 "small"、"default" 或 "large"。
	type: String as PropType<"" | "small" | "default" | "large">
modelValue：下拉选择框的绑定值，类型为字符串。
	type: String as PropType<any>
placeholder：下拉选择框的占位文本，类型为字符串。
	type: String as PropType<string>
clearable：是否显示清除图标，类型为布尔值，默认为 true。
	type: Boolean as PropType<boolean>
disabled：是否禁用下拉选择框，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
checkFirst：是否默认选中第一个选项，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
multiple：是否为多选模式，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
changeSensitive：是否为敏感的 change 模式，使用时可能会触发多次 change 事件，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
```

- 事件

```tsx
update:modelValue：当绑定值发生变化时触发，参数为新的绑定值。  //用于v-model
change：当选中的选项发生变化时触发，参数为新的选中值和对应的选项对象。
keydown：当按下回车键时触发，参数为一个包含 keyCode 属性的对象。
```

- 方法

```tsx
focus：使下拉选择框获得焦点。
getList：手动触发获取下拉选项数据。
blur：使下拉选择框失去焦点。
```

- 示例

```tsx
import { ElButton } from 'element-plus';
import { defineComponent, ref, onMounted } from 'vue'
import { DictSelect } from '~/src/core/components'
import { Dict } from '~/src/core/utils/dict';

export const cw_state = new Dict([
    {
        label: "占用",
        value: "1"
    },
    {
        label: "空床",
        value: "0"
    }
])
export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        const selectDict = ref("");
        const dictSelectRef = ref<any>();
        const list = ref()
        function focusDictSelect() {

            dictSelectRef.value?.focus();

        }
        return () => (
            <div>
                <div>
                    <DictSelect v-model={selectDict.value} ref={dictSelectRef} style={{ margin: '16px' }} dict={cw_state} clearable size={"large"}></DictSelect>
                </div>
                <div>
                    <ElButton onClick={focusDictSelect}>Focus DictSelect</ElButton>
                </div>
            </div>
        )
    }
})
```



#### TableSelect 表格下拉选择框

<img src=".\img\8.jpg" align="left" />

- 参数

```tsx
1.dict（必需）：字典名，类型为 TDictStrType | ITableDict。
	type: String as PropType<TDictStrType | ITableDict>
    	type TDictStrType = '',        	
        export interface ITableDict {
            /** 弹窗标题 */
            title: string,
            /** 是否每次重新从网络获取 */
            net?: boolean,
            /** 选完显示的字段 */
            showLabel: string,
            /** 结果 */
            showValue: string,
            /** 表格列配置 */
            column: TableColumn<any>[],
            /** 数据集 */
            dataFlag: Dict,
            /** 表格其他属性 */
            ldTableConfig?: ILdTableDfConfig<any>
        }
2.getList（可选）：获取数据的函数，类型为 () => Promise<any>。
	type: Function as PropType<() => Promise<any>>
3.modelValue（可选）：输入框的值，类型为 any。
	type: Object as PropType<any>
4.clearable（可选）：是否支持清空，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
5.checkbox（可选）：复选框响应式数据，类型为 any[]。
	type: Object as PropType<any[]>
6.tree（可选）：是否树形结构，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
7.multy（可选）：是否允许多选，类型为 boolean，默认为 false。
	type: Object as PropType<boolean>
8.readonly（可选）：是否只读，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
9.focusType（可选）：聚焦位置，类型为 "input" | "table"，默认为 "input"。
	type: String as PropType<"input" | "table">
10.size（可选）：组件大小，类型为 "" | "default" | "small" | "large"。
	type: String as PropType<"" | "default" | "small" | "large">
11.showType（可选）：显示类型，类型为 "twoInput" | "slot" | "hide" | "input"，默认为 "input"。
	type: String as PropType<"twoInput" | "slot" | "hide" | "input">
12.isDbClick（可选）：是否双击触发选中，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
13.width（可选）：组件宽度，类型为 number。
	type: Number as PropType<number>
14.setSingleSelect（可选）：设置单选，类型为 any。
	type: Object as PropType<any>
15.placeholder（可选）：输入框占位符，类型为 string，默认为 "请选择"。
	type: String as PropType<string>
16.singleChooseArr（可选）：设置单选互斥的数组组，且在多选时使用，类型为 any[][]，默认为 []。
	type: Object as PropType<any[][]>
```

- 示例1

```tsx
import { defineComponent, ref } from 'vue'
import { TableSelect } from '~/src/core/components'
import { Dict, ITableDict } from '~/src/core/utils/dict'
import request from "~/src/core/request";

export const stu_dict = new Dict(
    [
        {
            XM: 'li',
            XH: '2019230',
            AGE: 18,
            label: 'li',
            value: '2019230'
        },
        {
            XM: 'zheng',
            XH: '2019231',
            AGE: 19,
            label: 'zheng',
            value: '2019231'
        },
        {
            XM: 'chen',
            XH: '2019232',
            AGE: 20,
            label: 'chen',
            value: '2019232'
        }
    ]
)

export const table_zl_pro: ITableDict = {
    title: '诊疗项目',
    showLabel: 'label',
    showValue: 'value',
    column: [
        {
            dataKey: 'XM',
            title: '姓名',
            type: "html"
        },
        {
            dataKey: 'XH',
            title: '学号',
            type: "html"
        },
        {
            dataKey: 'AGE',
            title: '年龄',
            type: "html"
        },
    ],
    dataFlag: stu_dict,
}

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        return () => (
            <div>
                <TableSelect dict={table_zl_pro} ></TableSelect>
            </div>
        )
    }
})
```

- 示例2

```tsx
import { defineComponent, ref } from 'vue'
import { TableSelect } from '~/src/core/components'
import { Dict, ITableDict } from '~/src/core/utils/dict'
import request from "~/src/core/request";
/** 床位费获取 */
export function getZlPro(query?: any) {
    return request<IZlPro[]>({
        url: '/ld/common/dict/BedWork?method=getZlPro',
        method: 'post',
        data: {
            ...query
        }
    })
}

export interface IZlPro {
    /** 最后更新人 */
    ZHGXR: string,
    /** 组套判别 -1不参与 0减半 1只收一次 */
    ZTPB: string,
    /** 儿童价格 */
    CHILDREN_PRICE: number,
    /** 诊疗项目ID */
    ZLXM: string,
    /** 医嘱可用标志0否1是  是否可用于医生开医嘱 */
    YZBZ: string,
    /** 特需项目  0允许1不允许 */
    TSXM: string,
    /** 最大限量 */
    ZDYL: number,
    /** 医保费单个记录上传 */
    MISINGLEUP: string,
    /** 自定义码 */
    ZDYM: string,
    /** 最后更新人 */
    ZHGXSJ: string,
    /** 诊疗项目分类ID */
    ZLXMFL_ID: string,
    /** 自助设备允许查询标志 0允许1不允许 */
    ZZCX: string,
    /** 单位id */
    DW_ID: string,
    /** 价格 */
    PRICE: number,
    /** 单位名称 */
    DW_MC: string,
    /** 删除标志 */
    DEL_FLAG: string,
    /** 诊疗项目ID */
    ZLXM_ID: string,
    /** 序号 */
    ROW_ID: number,
    /** 手术室执行材料 */
    ISOPER: string,
    /** 性别限制 0不限制,1限男性使用,2限女性使用 */
    XBXZBZ: string,
    /** 诊疗项目名称 */
    ZLXM_MC: string,
    /** 审批项目(特检费用针对部分公医病人需要先审批后计费)(职工保健科维护) */
    SPXM: string,
    /** 费用报销类型ID */
    FYBXLX_ID: string,
    /** 收费类型名称 */
    SFLX_MC: string,
    /** 创建人 */
    CJR: string,
    /** 收费类型名称 */
    SFLX_ID: string,
    /** 住院收费类型ID */
    ZYSFLX_ID: string,
    /** 门诊收费类型名称 */
    MZSFLX_MC: string,
    /** 门诊收费类型ID */
    MZSFLX_ID: string,
    /** 住院收费类型名称 */
    ZYSFLX_MC: string,
    /** 市场调节  0允许1不允许' */
    SCTJ: string,
    /** 医保限制使用标志 */
    YBXZSYBZ: string,
    /** 创建时间 */
    CJSJ: string,
    /** 拼音码 */
    PYM: string,
    /** 医保适用范围 */
    REMARK2: string,
    /** 儿童年龄上限 */
    CHILDOLDUP: number,
    /** 文件编号 */
    WJBH: string,
    /** 门诊住院使用标志   1门诊， 2住院 */
    MZZY_FLAG: string,
    /** 医保代码 */
    YBDM: string,
    /** 最小限量 */
    ZXYL: number,
    /** icu不允许收费0否1是 */
    ICU_BYXSF: string
}

export const dict_dict_zl_pro = new Dict(async (query?: any) => {
    const json = await getZlPro({
        "pageStart": "1",
        "pageSize": "99999", //页数
        "flId": "1109,1102", //分类ID
        "delFlag": "0", //删除标志
        ...query
    })
    return json.data.map((e) => {
        return {
            ...e,
            pym: e.PYM,
            label: e.ZLXM_MC,
            value: e.ZLXM_ID,
            copyZLXM_MC: e.ZLXM_MC
        }
    })
})

export const table_zl_pro: ITableDict = {
    title: '学生',
    showLabel: 'label',
    showValue: 'value',
    column: [
        {
            dataKey: 'copyZLXM_MC',
            title: '项目名称',
            type: "html"
        },
        {
            dataKey: 'pym',
            title: '拼音码',
            type: "html"
        },
        {
            dataKey: 'PRICE',
            title: '单价',
            type: "html"
        },
    ],
    dataFlag: dict_dict_zl_pro,
}

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        return () => (
            <div>
                <TableSelect dict={table_zl_pro} ></TableSelect>
            </div>
        )
    }
})
```



#### TablePulldown  向下弹出式的表格选择器
类似TableSelect

<img src=".\img\9.jpg" align="left" />

- 参数

```tsx
1.dict（必需）：字典名，类型为 TDictStrType | ITableDict。
	type: String as PropType<TDictStrType | ITableDict>
    	type TDictStrType = '',        	
        export interface ITableDict {
            /** 弹窗标题 */
            title: string,
            /** 是否每次重新从网络获取 */
            net?: boolean,
            /** 选完显示的字段 */
            showLabel: string,
            /** 结果 */
            showValue: string,
            /** 表格列配置 */
            column: TableColumn<any>[],
            /** 数据集 */
            dataFlag: Dict,
            /** 表格其他属性 */
            ldTableConfig?: ILdTableDfConfig<any>
        }
2.getList（可选）：获取数据的函数，类型为 () => Promise<any>。
	type: Function as PropType<() => Promise<any>>
3.modelValue（可选）：输入框的值，类型为 any。
	type: Object as PropType<any>
4.clearable（可选）：是否支持清空，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
5.checkbox（可选）：复选框响应式数据，类型为 any[]。
	type: Object as PropType<any[]>
6.tree（可选）：是否树形结构，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
7.multy（可选）：是否允许多选，类型为 boolean，默认为 false。
	type: Object as PropType<boolean>
8.readonly（可选）：是否只读，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
9.focusType（可选）：聚焦位置，类型为 "input" | "table"，默认为 "input"。
	type: String as PropType<"input" | "table">
10.size（可选）：组件大小，类型为 "" | "default" | "small" | "large"。
	type: String as PropType<"" | "default" | "small" | "large">
11.showType（可选）：显示类型，类型为 "twoInput" | "slot" | "hide" | "input"，默认为 "input"。
	type: String as PropType<"twoInput" | "slot" | "hide" | "input">
12.isDbClick（可选）：是否双击触发选中，类型为 boolean，默认为 false。
	type: Boolean as PropType<boolean>
13.width（可选）：组件宽度，类型为 number。
	type: Number as PropType<number>
14.setSingleSelect（可选）：设置单选，类型为 any。
	type: Object as PropType<any>
15.placeholder（可选）：输入框占位符，类型为 string，默认为 "请选择"。
	type: String as PropType<string>
16.singleChooseArr（可选）：设置单选互斥的数组组，且在多选时使用，类型为 any[][]，默认为 []。
	type: Object as PropType<any[][]>
```



#### TablePulldownVxeTable

类似TablePulldown

<img src=".\img\10.jpg" align="left" />



#### DictText

//根据value 匹配dict的label文本并显示label 

如果未匹配到会在控制台打印未找到数据

<img src=".\img\11.jpg" align="left" />

```tsx
import { defineComponent } from 'vue'
import { DictSelect, DictText, TablePulldown, TablePulldownVxeTable } from '~/src/core/components'
import { Dict, ITableDict } from '~/src/core/utils/dict'

export const stu_dict = new Dict(
    [
        {
            XM: 'li',
            XH: '2019230',
            AGE: 18,
            label: 'li',
            value: '2019230'
        },
        {
            XM: 'zheng',
            XH: '2019231',
            AGE: 19,
            label: 'zheng',
            value: '2019231'
        },
        {
            XM: 'chen',
            XH: '2019232',
            AGE: 20,
            label: 'chen',
            value: '2019232'
        }
    ]
)

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        return () => (
            <div>
                <DictText dict={stu_dict} modelValue='2019230'></DictText>
            </div>
        )
    }
})
```



- 参数

```tsx
1.dict（必需）：字典名，类型为 TDictStrType | Dict
2.modelValue(必需)：value值，类型为 String
```



#### LdCheckBox

- 参数

```tsx
1.modelValue（可选）：复选框的值，类型为字符串。   //未选中为'0' ，选中为'1'
	type: String as PropType<string>
2.readonly（可选）：复选框是否为只读状态，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
3.label（可选）：复选框的标签文本，类型为字符串。
	type: String as PropType<string>
4.mr（可选）：复选框与其他元素之间的右边距，类型为字符串。
	type: String as PropType<string>

```

- 事件

```tsx
1.update:modelValue：当复选框的值发生变化时触发，用于更新 modelValue 属性的值。//用于v-model
2.change：当复选框的值发生变化时触发，可以用于执行自定义的逻辑。
```

- 暴露方法

```tsx
1.focus()：用于设置复选框的焦点
```

```tsx
import { defineComponent, ref } from 'vue'
import { LdCheckbox} from '~/src/core/components'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        const checkboxValue = ref('0') // 初始值设置为 '0'，表示未选中
        function handleSubmit() {
            if (checkboxValue.value === '1') {
              // 复选框被选中时的处理逻辑
              console.log('学生复选框已选中')
            } else {
              // 复选框未被选中时的处理逻辑
              console.log('学生复选框未选中')
            }
          }
        return () => (
          <div>
  			<LdCheckbox v-model={checkboxValue.value} label="学生" readonly={true} mr="10px"></LdCheckbox>
		  </div>
        )
    }
})
```



#### Model 渲染对象/函数

渲染组件内model参数,可配合useLdVxeTable使用

```tsx
1.model（必需）：模型数据，类型为任意对象或函数。
	type: Object as PropType<any>
2.dict（可选）：字典数据，类型为字符串数组。
	type: Array as PropType<TDictStrType[]>
3.overflow（可选）：内容溢出行为，可选值为 "hidden" 或 "visible"，默认为 "hidden"。
	type: String as PropType<"hidden" | 'visible'>
4.isTalbe（可选）：是否为表格组件，类型为布尔值，默认为 false。
	type: Boolean as PropType<boolean>
```

```tsx
当model为函数，渲染函数返回的对象内的Component属性
当model为对象，渲染对象内Component属性
```



- 示例1

```tsx
import { defineComponent } from 'vue'
import { Model } from '~/src/core/components'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {
        const VNode2 = () => (
            <p onClick={() => alert('hello p2')}>This is VNode2</p>
        )

        const modelData = {
            component: VNode2
        }
        const modelData1 = () => ({
            component: VNode2
        })
        return () => (
            <div>
                <Model model={modelData}></Model>
                <Model model={modelData1}></Model>
            </div>
        )
    }
})
```

- 示例2

```tsx
import { defineComponent, h } from 'vue'
import { useLdVxeTable } from '~/src/core/model/hooks';
import { DictSelect, LdForm, LdFormItem, Model } from "~/src/core/components";
import { ElButton } from 'element-plus';

export default defineComponent({

    setup(props, { slots, expose, emit, attrs }) {
        interface testIN {
            id?: number,
            XM?: string,
            XH?: string,
        }
        const testData = [
            {
                id: 1,
                XM: '张三',
                XH: '2019200'
            },
            {
                id: 2,
                XM: '李四',
                XH: '2019201'
            }
        ]
        const table = useLdVxeTable<testIN>({
            selection: true,
            async getList() { return testData },
            column: [
                {
                    width: 100,
                    title: '编号',
                    dataKey: 'id',
                },
                {
                    width: 100,
                    title: "姓名",
                    dataKey: "XM"
                },
                {
                    width: 100,
                    title: "学号",
                    dataKey: 'XH'
                },
            ]
        }, (obj: any) => {
            switch (obj.type) {
                case "selectChange":
                    break;
                default:
                    break;
            }
        })
        console.log(table.getRowRef.length);
        return () => (
            <div>
                    <Model model={table}></Model>
            </div>
        )
    }
})
```



#### ModelAsync

 与useLdVxeTableAsync一起使用

```tsx
import { ElButton, ElInput } from 'element-plus';
import { defineComponent, ref } from 'vue';
import { ModelAsync } from '~/src/core/components';
import { useLdVxeTableAsync } from '~/src/core/hooks';
import { useLdVxeTable } from '~/src/core/model/hooks';
import { sleep } from '~/src/core/utils';
import { apiDataToWebColumns } from './renderRule';

export default defineComponent({
    setup() {

        const tableAsync = useLdVxeTableAsync<any>(async (getTable) => {

            const column = apiDataToWebColumns([
                {
                    dataKey: 'data',
                    title: "日期时间",
                    width: 200,
                    sortable: true
                },
                {
                    dataKey: 'ruliang',
                    title: "入量",
                    children: [
                        {
                            title: "入量名称",
                            dataKey: "rlmc",
                            width: 100,
                            renderType: 'input_1'
                        },
                        {
                            title: "量",
                            dataKey: "l",
                            width: 100,
                            renderType: 'input_num'
                        },
                        {
                            title: "单位",
                            dataKey: "dw",
                            width: 100,
                            renderType: 'input_1'
                        }
                    ]
                }
            ], getTable)

            await sleep(1000);

            return useLdVxeTable({
                column,
                vxeConfig: {
                    toolbarConfig: {
                        slots: {
                            buttons: () => {
                                return [<ElButton onClick={async () => {
                                    const newRecord = {};
                                    const retData = await tableAsync.getTable()?.getTableRef().value?.insertAt(newRecord, -1);
                                    await tableAsync.getTable()?.getTableRef().value?.setEditRow(retData?.row);
                                }}>插入一行</ElButton>]
                            }
                        }
                    }
                },
                getList: async () => {
                    return []
                }
            });
        })

        return () => (
            <>
                <ModelAsync model={tableAsync}></ModelAsync>
            </>
        );
    }
});

```

```tsx
1.model（必需）：模型数据，类型为useLdVxeTableAsync的返回值
	 type: Object as PropType<TUseLdVxeTableAsyncReturn>,
```



#### ParentRoute

等同于<RouterView/>



#### SvgIconAl 图标

- 参数

```tsx
1.icon（必需）：图标的名称，类型为字符串（String）。
	type: String as PropType<String>
2.color（可选）：图标的颜色，类型为字符串（String）。
	type: String as PropType<String>
3.size（可选）：图标的大小，默认值为 "1em"，类型为字符串（String）。
	type: String as PropType<String>
```

- 示例

```tsx
<SvgIconAl icon={'icon-sousuo1'} color={'red'} size={'3em'} ></SvgIconAl>
```

图标库：src\assets\iconfont\iconfont.json

需要在font_class前加上 icon-



#### Title标题





### 布局

#### LdBox

##### 参数

```tsx
props: {
        /** 上下左右类型 */
        type: {
            type: String as PropType<"top-bottom" | "left-right">,
            required: false,
            default: "top-bottom"
        },
        showSplit:{  // 显示表尾
          type: Boolean as PropType<boolean>,
          required: false,
          default: true
        }
},
```



#### LdBoxItem

参数

```typescript
props: {
    /** 大小 - 百分比 */
    size: {
      type: Number as PropType<number | string>,
      required: true
    },
    minSize: {
      type: Number as PropType<number | string>,
      required: false
    },
    maxSize: {
      type: Number as PropType<number | string>,
      required: false
    }
},
```







##  泛用类型

### dict

```tsx
export class Dict {
    /** 字典唯一id */
    private id;
    /** 字典数据 */
    private data;
    /** 不缓存字典 */
    private uncached = false;
    /**
     * * 字典构造方法
     * @param dictData 字典数据
     * @param _uncached 是否不缓存
     */
    constructor(dictData: SelectDict, _uncached?: boolean) {
        this.id = nanoid();
        this.data = dictData
        if (typeof _uncached !== 'undefined') {
            this.uncached = _uncached
        }
    }
    
	/**
	type SelectDict = ((...args: any) => (Promise<{ label: string, value: string, [key: string]: any }[]>))
	| {
		label: string,
		value: string|number,
		[key: string]: any
	}[]
	*/
    
    /** 获取静态data数据 - 非异步 */
    getStatic() {
        if (typeof this.data === "function") {
            throw new Error("静态字典数据获取失败，该字典为异步字典")
            return null;
        }
        return this.data as {
            label: string,
            value: string | number,
            [key: string]: any
        }[]
    }

    /** 获取字典数据 - 获取完后不缓存到本地 */
    async get(...arg: any[]) {
        if (typeof this.data === "function") {
            return await this.data(...arg)
        } else {
            return this.data
        }
    }

    /** 获取字典数据 - 获取完后缓存到本地 */
    async getCache(query?: any) {

        if (this.uncached) {
            return await this.get(query);
        }

        let data = dictStorage.get(this.id);

        // 处理两处地方同时请求字典只发送一次接口请求
        if (dictStorage.getLoading(this.id)) {
            await sleep(1000);
            data = await this.getCache(query);
            return data;
        }

        if (!data) {
            dictStorage.setLoading(this.id, true)
            data = await this.get(query);
            dictStorage.setLoading(this.id, false)
        }
        dictStorage.set(this.id, data)
        return data
    }

    /** 刷新字典 */
    async refresh() {
        if (this.id) {
            dictStorage.remove(this.id);
        }
        return await this.getCache();
    }
}
```

label与value是必填参数

- 示例1 静态

```tsx
export const stu_dict = new Dict(
    [
        {
            XM: 'li',
            XH: '2019230',
            AGE: 18,
            label: 'li',
            value: '2019230'
        },
        {
            XM: 'zheng',
            XH: '2019231',
            AGE: 19,
            label: 'zheng',
            value: '2019231'
        },
        {
            XM: 'chen',
            XH: '2019232',
            AGE: 20,
            label: 'chen',
            value: '2019232'
        }
    ]
)
```

- 示例2 异步获取

```tsx
/** 床位费获取 */
export function getZlPro(query?: any) {
    return request<IZlPro[]>({
        url: '/ld/common/dict/BedWork?method=getZlPro',
        method: 'post',
        data: {
            ...query
        }
    })
}

export const dict_dict_zl_pro = new Dict(async (query?: any) => {
    const json = await getZlPro({
        "pageStart": "1",
        "pageSize": "99999", //页数
        "flId": "1109,1102", //分类ID
        "delFlag": "0", //删除标志
        ...query
    })
    return json.data.map((e) => {
        return {
            ...e,
            pym: e.PYM,
            label: e.ZLXM_MC,
            value: e.ZLXM_ID,
            copyZLXM_MC: e.ZLXM_MC
        }
    })
})
```

