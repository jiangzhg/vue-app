<template>
<div id="vue_det">
	<el-input
		type="textarea"
		id="localSessionDescription"
		autosize
		placeholder="Browser base64 Session Description"
		v-model="textarea1">
	</el-input>
	<div style="margin: 20px 0;"></div>
	<el-input
		type="textarea"
		id="remoteSessionDescription"
		:autosize="{ minRows: 2, maxRows: 2}"
		placeholder="Golang base64 Session Description"
		v-model="textarea2">
	</el-input>
	<div style="margin: 20px 0;"></div>
	<el-button onclick="window.startSession()" :disabled="isAble"
		type="primary" >Start to play</el-button>
	<div style="margin: 20px 0;"></div>
	<br />
	Video<br />
	<div id="remoteVideos"></div> <br />
	Logs<br />
	<div id="logdiv"></div>
</div>
</template>


<script>

export default {
	data() {
		return {
		textarea1: '',
		textarea2: '',
		isAble: false
		}
	},
}

let pc = new RTCPeerConnection({
	iceServers: [
		{
			//urls: 'stun:stun.l.google.com:19302'
			urls: 'stun:localhost:3478'
		}
	]
})

let log = msg => {
	document.getElementById('logdiv').innerHTML += msg + '<br>'
}

pc.ontrack = function (event) {
	var el = document.createElement(event.track.kind)
	el.srcObject = event.streams[0]
	el.autoplay = true
	el.controls = true

	document.getElementById('remoteVideos').appendChild(el)
}

pc.oniceconnectionstatechange = () => log(pc.iceConnectionState)
pc.onicecandidate = event => {
	if (event.candidate === null) {
		var local_value = btoa(JSON.stringify(pc.localDescription))
		document.getElementById('localSessionDescription').value = local_value
		pc.oniceconnectionstatechange
	}
}

// Offer to receive 0 audio, and 1 video tracks
// pc.addTransceiver('audio', {'direction': 'recvonly'})
pc.addTransceiver('video', {'direction': 'recvonly'})
//pc.addTransceiver('video', {'direction': 'recvonly'})
pc.createOffer().then(d => pc.setLocalDescription(d)).catch(log)

window.startSession = () => {
	let sd = document.getElementById('remoteSessionDescription').value
	if (sd === '') {
		return alert('Session Description must not be empty')
	}

	try {
		pc.setRemoteDescription(new RTCSessionDescription(JSON.parse(atob(sd))))
	} catch (e) {
		alert(e)
	}
}

</script>

