```ts

// cycloid new project --spa --monorepo --
// cycloid new action --widget
// cycloid new action
// cycloid new state --widget
// cycloid new state global
// cycloid new computed
// cycloid new widget --sceeen=
// cycloid new screen

function CycloidRoot() {
  return <></>
}

const todoWidgetStatus = createState<'idle'|'loading'>({
  default: 'idle',
  effect({value, prevValue}) {
    // 监听数据上报；打印日志等
  }
})

async function openUserInfoPanel() {
  const {modal} = getGlobalState()
  const setTodoWidgetStatus = setCycloidState(todoWidgetStatus)
  const fullname = getFullname()
  const nextState = await modal.show('userPanel',{
    title:'用户面板',
    content: fullname
  })
  if (nextState.modifired) {
    setTodoWidgetStatus('loading')
    await updateUserInfo(nextState.data)
    setTodoWidgetStatus('idle')
  }
}

const {getFullname, useFullname} = computed(()=> {
  const {user} = getGlobalState()
  return `${user.firstName}·${user.secondName}`
})

// 当组件销毁时，自动清理内部的 state，包括子组件的。

function Todo() {
  const fullname = useFullname()
  const todoWidgetStatusValue = useCycloidState(todoWidgetStatus)

  if (todoWidgetStatusValue==='loading') <Loading></Loading>
  return <HStack width='100px' height="100px" onClick={openUserInfoPanel}>{
  fullname
  }</HStack>
}

```
