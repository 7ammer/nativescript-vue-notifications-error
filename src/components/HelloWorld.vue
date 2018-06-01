<template>
	<Page class="page">
		<ActionBar class="action-bar" title="Hello world"></ActionBar>

		<StackLayout class="hello-world">
			<Button class="btn btn-primary" @tap="requestPermission" text="requestPermission"/>
			<Button class="btn btn-primary" @tap="schedule" text="schedule"/>
		</StackLayout>
	</Page>
</template>

<script>
	import * as LocalNotifications from "nativescript-local-notifications";

	export default {
		mounted() {
			LocalNotifications.addOnMessageReceivedCallback(
				function (notification) {
					console.log("ID: " + notification.id);
					console.log("Title: " + notification.title);
					console.log("Body: " + notification.body);
					alert(notification.title);
				}
			).then(
				function () {
					console.log("Listener added");
				}
			)
		},
		methods: {

			schedule() {
				alert('wait 5 seconds and click on notification');
				LocalNotifications.schedule([{
					id: 1,
					title: "test title",
					body: "test body",
					ticker: "test ticker",
					at: new Date(new Date().getTime() + (5 * 1000))
				}]).then(() => {
					console.log("Notification scheduled");
				}, (error) => {
					console.log("ERROR", error);
				});
			},
			requestPermission() {
				if (LocalNotifications.hasPermission()) alert('already has permission');
				else LocalNotifications.requestPermission().then(
					function (granted) {
						alert("Permission granted? " + granted);
						console.log("Permission granted? " + granted);
					}
				)
			}
		},
		data() {
			return {
			};
		},
	};
</script>

<style scoped>
	.hello-world {
		margin: 20;
	}
</style>
