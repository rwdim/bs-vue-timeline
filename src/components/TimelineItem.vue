<template>
<b-row no-gutters class="justify-content-around align-items-start timeline-nodes" :class="customClass">
	<b-col cols="9" md="5" offset="2" offset-md="0" order="3" order-md="1" class="timeline-content">
		<h3 class="text-light" v-html="itemTimeline.title"></h3>
		<h2 v-if="itemTimeline.subtitle" v-html="itemTimeline.subtitle"></h2>
		<p v-html="itemTimeline.content"></p>
	</b-col>
	<b-col cols="1" md="1" offset="1" offset-md="0" order="1" class="timeline-image d-flex justify-content-center mx-md-4">
		<img v-if="itemTimeline.image" v-bind:src="'img/' + itemTimeline.image" :alt="itemTimeline.title">
	</b-col>
	<b-col cols="9" md="5" order="2" offset="1" offset-md="0" order-md="3" class="py-3 timeline-date">
		{{ itemTimeline.from | FormatedDate }} <span v-if="itemTimeline.to">- {{ itemTimeline.to | FormatedDate }}</span>
		<time v-if="itemTimeLine.to" v-html="TimeDifference(itemTimeline.from, itemTimeline.to)"></time>
	</b-col>
</b-row>
</template>


<script>
import moment from "moment";
export default {
	name: "TimelineItem",
	props: {
		customClass: String,
		separator: String,
		itemTimeline: {
			type: Object,
			default: () => ({})
		}
	},
	methods: {
		TimeDifference(from, to) {
			let formatedTime = '<span class="duration">';
			var yearsDiff = moment(to).diff(moment(from), "year");
			var monthsDiff = moment(to).diff(moment(from), "months") - yearsDiff * 12; //substract the full years as months
			// var momentDiff = moment(to).from(moment(from), true)
			let UsedSeparator = "";
			if(yearsDiff > 0 && monthsDiff > 0) {
				UsedSeparator = this.separator;
			}
			// Show years only if the duration is over one year and handle the pluralisation
			if(yearsDiff === 1) {
				formatedTime = formatedTime + " 1 year";
			} else if(yearsDiff > 1) {
				formatedTime = formatedTime + yearsDiff + " years";
			}
			// Show months only if the months are over 1 and handle the pluralisation
			if(monthsDiff === 1) {
				formatedTime = formatedTime + UsedSeparator + "1 month";
			} else if(monthsDiff > 1) {
				formatedTime = formatedTime + UsedSeparator + monthsDiff + " months";
			}
			formatedTime = formatedTime + "</span>";
			return formatedTime;
		}
	},
	filters: {
		FormatedDate: function(InputDate) {
			let momentDate = moment(InputDate);
			if(moment().isSame(momentDate, "month")) {
				return "Present time";
			} else {
				return momentDate.format("MM YYYY");
			}
		}
	}
};
</script>
