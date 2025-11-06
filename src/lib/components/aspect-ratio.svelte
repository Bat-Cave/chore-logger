<script lang="ts">
	let { children = () => {}, ratio, class: className } = $props();
</script>

<div style={`position: relative;width: 100%;padding-bottom: ${100 / ratio}%;`} class={className}>
	<div style="position: absolute; top: 0; right: 0; bottom: 0; left: 0;">
		{@render children()}
	</div>
</div>
