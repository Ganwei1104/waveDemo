<script>
export default {
  name: 'MenuItem',
  functional: true,
  props: {
    icon: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    },
    first: {
      type:String,
      defaule: ''
    }
  },
  render(h, context) {
    const { icon, title, first} = context.props
    const vnodes = []

    if (icon) {
      vnodes.push(<svg-icon icon-class={icon}/>)
    }

    if (title) {
      vnodes.push(<span slot='title' class='spanactive'>{(title)}</span>)
    }

    if(first) {
      vnodes.push(<span class='firstactive'>{(first)}</span>)
    }
    return vnodes
  }
}
</script>
