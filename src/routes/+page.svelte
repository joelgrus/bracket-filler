<script>
	import Canvas from '../lib/Canvas.svelte';
</script>

<p class="p-2 m-2">
	some friends of mine made a
	<a class="text-blue-500" href="https://recsys-madness.com/" target="_blank">bracket contest</a>,
	but they wanted you to fill in your bracket by hand in MS Paint, like a peasant
</p>

<p class="p-2 m-2">so I hacked this together</p>

<p class="p-2 m-2">I spent like zero time on UX / design, can you tell</p>

<p class="p-2 m-2">(I wanted to mess around with something like this for a while anyway)</p>

<p class="p-2 m-2">fill in your entries at the bottom and then click the download button.</p>

<Canvas />

<div>
	made by <a target="_blank" class="text-blue-600" href="https://twitter.com/joelgrus">@joelgrus</a>
</div>

<div>
	<a target="_blank" class="text-blue-600" href="https://github.com/joelgrus/bracket-filler"
		>github.com/joelgrus/bracket-filler</a
	>
</div>
