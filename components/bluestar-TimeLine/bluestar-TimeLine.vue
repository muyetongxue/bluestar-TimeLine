<template>
	<view class="time-line">
		<view class="time-content">
			<view class="time-wrap" :style="{width : width + 'vw'}">
				<view v-for="(value,index) in timeList.value"
							:key="index"
							:class="['time-node' , index === active ? 'active' : '' , index === active - 1 ? 'previous' : '']"
							:style="index === active ? styleObject : ''"
							@click="onChange(index)">
					<text class="day">{{ value.day }}</text>
					<text class="date">{{ value.formatDate }}</text>
					<text class="iconfont icon-xuanzhong selected" v-if="index === active"></text>
				</view>
			</view>
		</view>
	</view>
</template>

<script setup lang="ts">
import moment from 'moment'
import {onMounted, reactive, ref, toRaw, withDefaults} from "vue"

interface Props {
	beginDate : string,
	endDate : string,
	reverse? : boolean,
	color? : string
}

const props = withDefaults(defineProps<Props>(), {
	beginDate : '',
	endDate : '',
	reverse : false,
	color : '#0E75FC'
})

const width = ref(0)
const active = ref(0)
const timeList : any = reactive({value : []})
const styleObject : any = reactive({
	color : props.color,
	backgroundColor : props.color + '1A',
	borderColor : props.color
})

const emit = defineEmits<{
	(event : 'callback', obj : object) : object
}>()

onMounted(() => init())

const init = () => {
	const days = Math.abs(moment(props.beginDate).diff(moment(props.endDate), 'days')) + 1
	width.value = 97 / 7.5 * days
	timeList.value = getTimeList(days)
}

const onChange = (index : number) => {
	if (active.value !== index) active.value = index
	emit('callback', toRaw(timeList.value)[active.value])
}

const getTimeList = (days : number) => {
	let ab = moment(props.beginDate).format('YYYY-MM-DD').split('-'),
		ae = moment(props.endDate).format('YYYY-MM-DD').split('-'),
		db = new Date(),
		de = new Date(),
		arr = []
	db.setUTCFullYear(parseInt(ab[0]), parseInt(ab[1]) - 1, parseInt(ab[2]))
	de.setUTCFullYear(parseInt(ae[0]), parseInt(ae[1]) - 1, parseInt(ae[2]))
	let unixDb = db.getTime(),
		unixDe = de.getTime(),
		weeks = ['周日', '周一', '周二', '周三', '周四', '周五', '周六']
	for (let k = unixDb; k <= unixDe;) {
		let obj : object | any = {}
		obj.date = moment(k).format('YYYY-MM-DD')
		obj.formatDate = moment(k).format('MM.DD')
		obj.day = moment().format('YYYY-MM-DD') === obj.date ? '今天' : weeks[moment(obj.date).day()]
		arr.push(obj)
		k = k + 24 * 60 * 60 * 1000
	}
	days ? arr = arr.slice(0, days) : arr
	props.reverse ? arr.reverse() : arr
	return arr
}
</script>

<style lang="scss" scoped>
.time-line {
	width: 100%;
	padding: 0 vw(36);
	background-color: #FFFFFF;
	border-top: vw(2) solid rgba(0, 0, 0, 0.1);
	border-bottom: vw(2) solid rgba(0, 0, 0, 0.1);
	overflow-y: hidden;
	.time-content {
		overflow-x: scroll;
		-webkit-overflow-scrolling: touch;
		transform: translateZ(0);
		.time-wrap {
			//根据node长度和数量,自动计算该节点长度
			.time-node {
				position: relative;
				float: left;
				width: vw(97);
				padding: vw(30) 0;
				display: flex;
				flex-flow: column nowrap;
				align-items: center;
				font-size: vw(24);
				color: #2D3239;
				border: vw(3) solid transparent;
				border-right: none;
				border-radius: vw(8);
				.day, .date {
					margin: 0 0 vw(8) 0;
				}
				.selected {
					position: absolute;
					right: vw(-1);
					bottom: vw(-1);
					font-size: vw(48);
					z-index: 2;
					font-weight: normal;
					background-color: transparent;
				}
				&.active {
					font-weight: bold;
					border-width: vw(3);
					border-style: solid;
				}
				&:not(:last-child):after {
					content: "";
					position: absolute;
					right: 0;
					top: vw(30);
					width: vw(2);
					height: calc(100% - vw(60));
					background-color: #E1E5EC;
				}
				&.active:after, &.previous:after {
					display: none;
				}
			}
			&:after {
				content: "";
				display: table;
				clear: both;
			}
		}
	}
	::-webkit-scrollbar {
		height: 0 !important;
	}
}
</style>
