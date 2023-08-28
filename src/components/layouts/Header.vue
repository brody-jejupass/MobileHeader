<script setup lang="ts">
import { ref, onMounted } from 'vue'

const defineElement = (items: string[]) => Object.fromEntries(items.map((item: string) => [item, ref<HTMLElement>()]));
const el = defineElement([
	'gnb',
	'first',
	'last'
]);

const gnbScrollState = ref<string | boolean>();
const gnbScrollLeft = ref<number>();

const gnbScroll = () => {
	el.gnb.value ?
	el.gnb.value.scrollLeft <= 0 ?
	gnbScrollState.value = 'start' :
	Math.floor(el.gnb.value.scrollWidth - el.gnb.value.scrollLeft) <= window.outerWidth ?
	gnbScrollState.value = 'end' :
	gnbScrollState.value = 'center' :
	null;

	gnbScrollLeft.value = el.gnb.value?.scrollLeft
}

const gnbScrollMove = (btn: HTMLElement | undefined): void => {
	el.gnb.value?.scroll({left: btn?.offsetLeft, behavior: "smooth"})
}

onMounted(() => {
	el.gnb.value ?
	gnbScrollState.value = el.gnb.value.clientWidth < el.gnb.value.scrollWidth ?
	'start' :
	'' :
	null;
});
</script>

<template>
	<header id="header">
		<router-link to="/" class="logo">제주패스</router-link>

		<router-link to="" class="btn btn-mypage">마이페이지</router-link>
	</header>

	<div id="gnb">
		<ul :class="`gnb-list ${gnbScrollState}`" @scroll="gnbScroll" :ref="el.gnb">
			<li :ref="el.first">
				<router-link to="/ai"><span>AI검색<small>Beta</small></span></router-link>
				<p class="speechBubble" :style="{'left': `calc(0px - ${gnbScrollLeft}px)`}">
                    제주패스 AI검색<span>Beta</span>이<br>
                    새롭게 오픈했습니다!
                </p>
			</li>
			<li><router-link to="/rentercar">렌터카</router-link></li>
			<li><router-link to="/rentercarOversea">해외렌터카</router-link></li>
			<li><router-link to="/air">항공</router-link></li>
			<li><router-link to="/stay">숙박</router-link></li>
			<li><router-link to="/trip">트립</router-link></li>
			<li><router-link to="/trip">트립</router-link></li>
			<li><router-link to="/trip">트립</router-link></li>
			<li :ref="el.last"><router-link to="/cafe">카페패스</router-link></li>
		</ul>

		<button type="button" class="btn btn-prev" @click="gnbScrollMove(el.first.value)">이전</button>
		<button type="button" class="btn btn-next" @click="gnbScrollMove(el.last.value)">다음</button>
	</div>
</template>

<style>
#header {display: flex;flex-wrap: wrap;justify-content: space-between;align-items: center;position: relative;}
#header .logo {font-size: 0;padding: 0 20px;}
#header .logo:before {content: '';display: block;width: 110px;height: 20px;background-color: #ccc;}
#header .btn-mypage {display: flex;justify-content: center;align-items: center;width: 56px;height: 56px;font-size: 0;background-color: #ccc;}
#header .btn-mypage:before,
#header .btn-mypage:after {content: '';width: 20px;box-sizing: border-box;}
#header .btn-mypage:before {height: 16px;border-top: 2px solid #202020;border-bottom: 2px solid #202020;}
#header .btn-mypage:after {position: absolute;border-top: 2px solid #202020;}

#gnb {position: sticky;top: 0;width: 100%;}
#gnb .btn {display: none;justify-content: center;align-items: center;position: absolute;top: 0;width: 32px;height: 48px;font-size: 0;background-color: transparent;}
#gnb .btn:before {content:'';display: flex;justify-content: center;align-items: center;width: 20px;height: 20px;font-weight: 700;font-size: 14px;color: #000;border: 1px solid #ededed;border-radius: 50%;background: #fff;}
#gnb .btn-prev {left: 0;}
#gnb .btn-prev:before {content: '<';}
#gnb .btn-next {right: 0;}
#gnb .btn-next:before {content: '>';}
#gnb:has(.end, .center) .btn-prev {display: flex;}
#gnb:has(.start, .center) .btn-next {display: flex;}
#gnb .gnb-list {display: flex;flex-wrap: nowrap;background-color: #000;overflow: visible;overflow-x: auto;scroll-snap-stop: always;scroll-snap-type: x mandatory;}
#gnb .gnb-list::-webkit-scrollbar {display: none;}
#gnb .gnb-list > li a {display: flex;align-items: center;position: relative;height: 48px;white-space: nowrap;padding: 0 8px;scroll-snap-align: start;}
#gnb .gnb-list > li:first-child > a {padding-left: 20px;}
#gnb .gnb-list > li:last-child > a {padding-right: 20px;}
#gnb .gnb-list > li a.router-link-active {color: #63a1ff;}
#gnb .gnb-list > li a span {display: flex;align-items: start;}
#gnb .gnb-list > li a span small {font-weight: 600;font-size: 12px;color: #e83667;}
#gnb .gnb-list > li .speechBubble {position: absolute;z-index: 200;top: 38px;left: 10px;font-weight: 600;font-size: 12px;line-height: 15px;color: #fff;padding: 8px 21px 8px 12px;border-radius: 25px;background-color: #63a1ff;opacity: 0;animation:gelatine 3s}
#gnb .gnb-list > li .speechBubble span {display: inline-block;font-weight: 600;font-size: smaller;line-height: 1;vertical-align: top;color: #e63668;margin-top: 1px;}
#gnb .gnb-list > li .speechBubble:after {content: "";position: absolute;top: -8px;left: 35px;border: 4px solid rgba(0,0,0,0);border-bottom-color: #63a1ff;}

@keyframes gelatine{
  from, to{transform:scale(1, 1);opacity:0}
  10%{transform:scale(0.9, 1.1);opacity:1}
  15%{transform:scale(1.1, 0.9);opacity:1}
  20%{transform:scale(0.95, 1.05);opacity:1}
  25%, 90%{transform:scale(1, 1);opacity:1}
}
</style>
