<script setup>
import { computed, reactive, ref } from "vue";
import guizhouBoundary from "./data/guizhou-boundary.json";
import guizhouOutline from "./data/guizhou-outline.json";
import ruralTourismBoard from "./data/rural-tourism-board.json";
import {
  BarChart3,
  Blocks,
  Building2,
  Download,
  ChevronDown,
  ChevronLeft,
  CalendarDays,
  ChevronRight,
  Database,
  FolderTree,
  Hotel,
  LayoutDashboard,
  Landmark,
  MapPinned,
  Search,
  Send,
  ShieldCheck,
  SlidersHorizontal,
  Sparkles,
  Star,
  Table2,
  UserCog,
  Users,
} from "@lucide/vue";

const navigation = [
  { id: "dashboard", label: "总览驾驶舱", icon: LayoutDashboard },
  { id: "resources", label: "资源管理", icon: FolderTree },
  { id: "systems", label: "业务系统目录", icon: Building2 },
  { id: "assets", label: "数据资产中心", icon: Database },
  { id: "query", label: "数据查询中心", icon: Search },
  { id: "services", label: "数据接入与共享", icon: Send },
  { id: "analysis", label: "数据看板", icon: BarChart3 },
  { id: "permissions", label: "权限管理", icon: ShieldCheck },
  { id: "governance", label: "治理与架构", icon: Blocks },
];

const viewParentMap = {
  "dashboard-todos": "dashboard",
  "dashboard-warnings": "dashboard",
  "dashboard-monitor": "dashboard",
  "resource-detail": "resources",
  "asset-detail": "assets",
  "metric-detail": "assets",
  query: "query",
  "access-detail": "services",
  "service-detail": "services",
  "share-detail": "services",
  "share-create": "services",
  "share-review": "services",
  "share-review-detail": "services",
  "shared-data": "services",
  "shared-data-detail": "services",
};

const field = (label, value, options = {}) => ({ label, value, ...options });
const section = (title, fields, options = {}) => ({ title, fields, ...options });

const regionPool = [
  { city: "贵阳市", county: "南明区" },
  { city: "贵阳市", county: "花溪区" },
  { city: "贵阳市", county: "观山湖区" },
  { city: "安顺市", county: "镇宁布依族苗族自治县" },
  { city: "安顺市", county: "西秀区" },
  { city: "遵义市", county: "汇川区" },
  { city: "遵义市", county: "赤水市" },
  { city: "六盘水市", county: "盘州市" },
  { city: "毕节市", county: "大方县" },
  { city: "毕节市", county: "织金县" },
  { city: "铜仁市", county: "江口县" },
  { city: "黔东南州", county: "雷山县" },
  { city: "黔东南州", county: "榕江县" },
  { city: "黔东南州", county: "镇远县" },
  { city: "黔南州", county: "荔波县" },
  { city: "黔南州", county: "都匀市" },
  { city: "黔西南州", county: "兴义市" },
  { city: "黔西南州", county: "兴仁市" },
  { city: "贵阳市", county: "乌当区" },
  { city: "遵义市", county: "播州区" },
];

const scenicNames = [
  "黄果树旅游区",
  "梵净山景区",
  "荔波小七孔景区",
  "龙宫景区",
  "百里杜鹃景区",
  "赤水丹霞旅游区",
  "织金洞景区",
  "马岭河峡谷景区",
  "万峰林景区",
  "镇远古城景区",
  "天河潭景区",
  "乌蒙大草原景区",
  "九洞天景区",
  "四洞沟景区",
  "娄山关景区",
  "杉木河景区",
  "黔灵山景区",
  "云台山景区",
  "桃源河景区",
  "格凸河景区",
];

const streetNames = [
  "青岩古镇休闲街区",
  "甲秀楼历史文化街区",
  "曹状元街历史文化街区",
  "青云路夜游街区",
  "多彩贵州城文旅街区",
  "遵义1935文化街区",
  "捞沙巷特色街区",
  "下司古镇休闲街区",
  "镇远古巷风情街区",
  "都匀石板街旅游街区",
  "榕江村超夜市街区",
  "西江苗寨风情街区",
  "肇兴侗寨鼓楼街区",
  "安顺儒林路文旅街区",
  "赤水河谷生活街区",
  "兴义万峰林集市街区",
  "花溪十字街美食街区",
  "乌当偏坡度假街区",
  "盘州古城休闲街区",
  "铜仁中南门历史街区",
];

const resortNames = [
  "万峰林旅游度假区",
  "乌蒙大草原旅游度假区",
  "双龙航空港旅游度假区",
  "赤水河谷旅游度假区",
  "荔波樟江旅游度假区",
  "肇兴侗寨旅游度假区",
  "百里杜鹃康养度假区",
  "龙里油画大草原度假区",
  "花溪高坡旅游度假区",
  "贵安山地运动度假区",
  "梵净山云舍旅游度假区",
  "马岭河峡谷康养度假区",
  "盘州妥乐银杏度假区",
  "镇远舞阳河旅游度假区",
  "都匀毛尖山旅游度假区",
  "安顺旧州温泉度假区",
  "兴仁放马坪旅游度假区",
  "赤水天鹅堡旅游度假区",
  "贵定金海雪山度假区",
  "遵义乌江寨旅游度假区",
];

const villageNames = [
  "西江千户苗寨",
  "肇兴侗寨",
  "加榜梯田",
  "妥乐古银杏村",
  "云舍村",
  "高荡村",
  "瑶山古寨",
  "石桥古法造纸村",
  "海龙屯山居村",
  "偏坡布依乡村旅游点",
  "中洞苗寨",
  "岜沙苗寨",
  "岩博村",
  "纳灰村",
  "阿妹戚托小镇",
  "杉坪村",
  "下纳灰布依村",
  "大同古镇乡村点",
  "旧州屯堡村落",
  "三宝侗寨",
];

const cultureNames = [
  "贵州省博物馆",
  "贵州省民族博物馆",
  "遵义会议纪念馆",
  "安顺市博物馆",
  "铜仁市博物馆",
  "毕节市博物馆",
  "黔东南州民族博物馆",
  "黔南州博物馆",
  "黔西南州博物馆",
  "六盘水市博物馆",
  "赤水市博物馆",
  "贵阳孔学堂文化馆",
  "贵州省图书馆",
  "多彩贵州非遗展示馆",
  "西江苗族文化展示馆",
  "肇兴侗族文化展示馆",
  "安顺屯堡文化体验馆",
  "贵州省美术馆",
  "贵安新区文化艺术中心",
  "贵州数字文化体验中心",
];

const hotelNames = [
  "荔波樟江部落大酒店",
  "黄果树迎宾酒店",
  "梵净山云舍精品民宿",
  "西江千户苗寨观景酒店",
  "都匀匀东国际酒店",
  "兴义万峰林牧云酒店",
  "赤水河谷山居酒店",
  "贵阳甲秀楼城景酒店",
  "遵义红城国际酒店",
  "安顺瀑乡度假酒店",
  "织金洞山地酒店",
  "镇远舞阳河精品客栈",
  "肇兴侗寨鼓楼民宿",
  "乌蒙大草原房车营地酒店",
  "龙里草原星空酒店",
  "黔灵山城市会客酒店",
  "贵安樱花湖会议中心酒店",
  "盘州妥乐银杏酒店",
  "毕节百里杜鹃温泉酒店",
  "铜仁中南门文旅酒店",
];

const agencyNames = [
  "贵州中青旅国际旅行社",
  "多彩贵州国际旅行社",
  "贵阳山地旅游旅行社",
  "遵义红色之旅旅行社",
  "安顺瀑乡旅行社",
  "黔东南民族风情旅行社",
  "黔南山水旅行社",
  "兴义峰林假日旅行社",
  "赤水河谷旅行社",
  "铜仁梵净文旅旅行社",
  "毕节乌蒙旅行社",
  "盘州康养旅行社",
  "镇远古城旅行社",
  "荔波樟江旅行社",
  "肇兴侗寨旅行社",
  "西江苗寨旅行社",
  "贵安新区会展旅行社",
  "花溪假日旅行社",
  "乌江寨文化旅行社",
  "贵阳入境旅游旅行社",
];

const guideNames = [
  "导游 杨莉",
  "导游 吴丹",
  "导游 赵敏",
  "导游 李莎",
  "导游 唐俊",
  "导游 刘娟",
  "导游 罗静",
  "导游 王宇",
  "导游 何婷",
  "导游 石磊",
  "导游 马会",
  "导游 张航",
  "导游 龙艳",
  "导游 陈思",
  "导游 彭琳",
  "导游 冉雪",
  "导游 胡波",
  "导游 梁倩",
  "导游 徐畅",
  "导游 韦宁",
];

const eventNames = [
  "村超赛事活动",
  "村BA系列赛事",
  "黄果树瀑布节",
  "多彩贵州文化旅游节",
  "贵阳路边音乐会",
  "梵净山徒步大会",
  "西江苗年节",
  "肇兴侗年活动",
  "遵义红色马拉松",
  "安顺屯堡文化周",
  "赤水河谷骑行赛",
  "百里杜鹃花季活动",
  "荔波小七孔泼水节",
  "贵安樱花季活动",
  "贵州避暑旅游推广季",
  "黔南山地旅游大会",
  "万峰林低空嘉年华",
  "黔东南民族文化周",
  "贵州非遗展演月",
  "乌江寨夜游节",
];

const businessNames = [
  "镇宁布依风味餐饮中心",
  "黄果树旅游商品旗舰店",
  "西江苗寨文创集市",
  "肇兴侗寨非遗工坊",
  "青岩古镇特色餐饮店",
  "贵阳甲秀文旅商业综合体",
  "赤水河谷自驾服务站",
  "梵净山游客集散服务中心",
  "荔波小七孔旅游购物店",
  "万峰林乡村骑行服务点",
  "都匀毛尖文旅体验店",
  "镇远古城夜游配套中心",
  "安顺屯堡文创经营点",
  "黔灵山景区餐饮服务站",
  "贵安新区露营装备店",
  "盘州乌蒙山地咖啡馆",
  "毕节杜鹃花季特产店",
  "铜仁中南门文旅消费点",
  "遵义红城旅游商品店",
  "榕江村超赛事消费驿站",
];

const cateringNames = [
  "镇宁布依风味餐饮中心",
  "青岩古镇特色餐饮店",
  "黔灵山景区餐饮服务站",
  "盘州乌蒙山地咖啡馆",
  "榕江村超赛事消费驿站",
  "贵阳老城苗味餐厅",
  "西江长桌宴体验馆",
];

const shoppingNames = [
  "黄果树旅游商品旗舰店",
  "西江苗寨文创集市",
  "肇兴侗寨非遗工坊",
  "荔波小七孔旅游购物店",
  "都匀毛尖文旅体验店",
  "安顺屯堡文创经营点",
  "毕节杜鹃花季特产店",
  "遵义红城旅游商品店",
];

const tourismServiceNames = [
  "贵阳甲秀文旅商业综合体",
  "赤水河谷自驾服务站",
  "梵净山游客集散服务中心",
  "万峰林乡村骑行服务点",
  "镇远古城夜游配套中心",
  "贵安新区露营装备店",
  "铜仁中南门文旅消费点",
];

const resourceGroups = [
  { id: "tourism-resources", label: "旅游资源目录", categories: ["景区", "街区", "度假区", "乡村旅游点"] },
  { id: "accommodation-resources", label: "住宿资源目录", categories: ["酒店民宿"] },
  { id: "cultural-resources", label: "文化资源目录", categories: ["文化场馆"] },
  { id: "tourism-subjects", label: "涉旅主体目录", categories: ["旅行社", "导游", "餐饮主体", "购物主体", "旅游服务主体"] },
];

const allResourceCategories = resourceGroups.flatMap((item) => item.categories);

const systemTypes = ["全部", "资源基础", "业务系统", "智能应用", "外部数据", "在建系统"];
const assetTypes = ["全部", "主数据资产", "指标资产", "标签资产", "知识资产", "服务资产"];
const assetNavigationItems = [
  { id: "asset-overview", label: "资产总览" },
  { id: "asset-master", label: "主数据资产", type: "主数据资产" },
  { id: "asset-indicators", label: "指标资产" },
  { id: "asset-tags", label: "标签资产", type: "标签资产" },
  { id: "asset-knowledge", label: "知识资产", type: "知识资产" },
  { id: "asset-services", label: "服务资产", type: "服务资产" },
];
const assetModuleTypeMap = Object.fromEntries(
  assetNavigationItems.filter((item) => item.type).map((item) => [item.id, item.type]),
);
const assetTypeModuleMap = Object.fromEntries(
  assetNavigationItems.filter((item) => item.type).map((item) => [item.type, item.id]),
);
const assetDirectoryModuleIds = Object.keys(assetModuleTypeMap);
const serviceTypes = ["全部", "API", "专题数据集", "指标服务", "视频服务", "智能服务"];

const serviceNavigationItems = [
  { id: "access", label: "数据接入" },
  { id: "share", label: "数据共享" },
];

const shareTertiaryItems = [
  { id: "share-list", label: "共享申请" },
  { id: "share-review", label: "共享审核" },
  { id: "shared-data", label: "共享清单" },
  { id: "share-calls", label: "数据调用" },
];

function regionAt(index) {
  return regionPool[index % regionPool.length];
}

function coord(base, index, step) {
  return (base + (index % 10) * step + Math.floor(index / 10) * step * 0.7).toFixed(4);
}

function maskedPhone(index) {
  return `13${(80 + (index % 10)).toString()}****${(1600 + index).toString().slice(-4)}`;
}

function buildScenicSections(item, index) {
  const scenicTypes = ["山岳型", "山岳瀑布型", "喀斯特型", "峡谷河谷型", "综合观光型"];
  const features = [
    "瀑布群景观、山地步道、观景平台",
    "喀斯特溶洞、峡谷栈道、夜游灯光",
    "民族文化体验、山地研学、生态观光",
  ];
  return [
    section("景区基础字段", [
      field("景区全称", item.name),
      field("景区别名", item.aliases, { full: true }),
      field("质量等级", item.level),
      field("营业状态", index % 6 === 0 ? "旺季高峰运营" : "正常营业"),
      field("景区类型", scenicTypes[index % scenicTypes.length]),
      field("景观特点", features[index % features.length], { full: true }),
      field("日均最大承载量", `${32000 + index * 1300}人次`),
      field("是否收费", item.chargeMode === "免费" ? "否" : "是"),
      field("是否预约", index % 4 === 0 ? "否" : "是"),
      field("景区知名度", index < 6 ? "全国知名" : "省内重点"),
      field("省份", item.province),
      field("市州", item.city),
      field("区县", item.county),
      field("详细地址", item.address, { full: true }),
      field("经度", item.longitude),
      field("纬度", item.latitude),
      field("海拔", `${780 + index * 32}米`),
      field("官方电话", `0851-${33000000 + 1203 + index}`),
      field("景区官网", `www.gzscenic${index + 1}.cn`),
      field("投诉电话", "12345 / 12318"),
    ]),
    section("介绍与服务信息", [
      field("官方介绍", `${item.name}是贵州重点旅游资源之一，纳入省级资源目录和重点景区客流监测范围。`, { full: true }),
      field("详细介绍", `${item.name}已关联客流、门票订单、停车场、LBS热度和视频监控等数据，支撑驾驶舱、应急指挥和智能问答应用。`, { full: true }),
      field("最佳游玩时间", ["3月至5月", "5月至10月", "7月至11月"][index % 3]),
      field("是否可夜游", index % 3 === 0 ? "可夜游" : "暂不开放"),
      field("优待政策", "老年人、儿童、学生、现役军人等可按政策享受免票或优惠。", { full: true }),
      field("温馨提示", "节假日高峰建议提前预约，关注景区实时客流与交通提示。", { full: true }),
      field("安全须知", "涉水、观景和高空步道区域请遵守景区引导，不跨越警戒线。", { full: true }),
      field("景区图集", ["主景区入口", "核心景观点", "游客服务中心", "停车场引导图"], { full: true }),
      field("综合评分", `${(4.5 + (index % 4) * 0.1).toFixed(1)} / 5`),
    ]),
  ];
}

function buildHotelSections(item, index) {
  return [
    section("注册主体信息", [
      field("单位名称", item.name),
      field("执照类型", "营业执照"),
      field("营业执照或同等效力照片", "已上传营业执照扫描件", { full: true }),
      field("注册单位名称", `${item.name}运营管理有限公司`),
      field("统一社会信用代码", `9152${(2000 + index).toString().padStart(4, "0")}MA6H****${(index + 13).toString().padStart(2, "0")}`),
      field("注册/批准机关名称", `${item.city}市场监督管理局`),
      field("其他批准机关名称", `${item.city}文化和旅游局`),
      field("工商登记时间", `201${index % 9}-0${(index % 8) + 1}-1${index % 9}`),
      field("当前经营状态", "正常经营"),
      field("注册地址", item.address, { full: true }),
      field("区划地址", `${item.province}-${item.city}-${item.county}`),
      field("区划代码（12位）", `5200${(100000 + index * 17).toString().slice(0, 8)}`),
      field("法定代表人姓名", `负责人${index + 1}`),
      field("法定代表人联系电话", maskedPhone(index)),
      field("法定代表人身份号码", `5227************${(20 + index).toString().slice(-2)}`),
    ], { note: "涉及个人信息的字段按门户权限规则脱敏展示。" }),
    section("经营主体信息", [
      field("经营单位名称", item.name),
      field("场所类别", item.name.includes("民宿") ? "民宿" : "住宿业"),
      field("注册类型", index % 3 === 0 ? "有限责任公司" : "个体工商户"),
      field("当前经营状态", item.chargeMode === "免费" ? "季节性营业" : "正常营业"),
      field("实际经营地址", `${item.address}${item.name.includes("民宿") ? "游客服务区旁" : "综合服务区"}`, { full: true }),
      field("实际经营区划地址", `${item.province}-${item.city}-${item.county}`),
      field("实际经营区划代码", `5200${(200000 + index * 21).toString().slice(0, 8)}`),
      field("经营负责人姓名", `经营负责人${index + 1}`),
      field("经营负责人联系电话", maskedPhone(index + 30)),
      field("经营负责人身份证号码", `5227************${(40 + index).toString().slice(-2)}`),
      field("经营负责人实际居住地", `${item.city}${item.county}城区`, { full: true }),
      field("行业代码", item.name.includes("民宿") ? "6120" : "6110"),
      field("房源名称", `${item.name}主楼`),
      field("房源使用权类别", index % 2 === 0 ? "自有" : "租赁"),
      field("房源类型", item.name.includes("民宿") ? "民宿" : "酒店"),
      field("房源性质", "商业经营用房"),
      field("房源面积", `${5600 + index * 260}平方米`),
      field("管辖单位名称", `${item.city}文化和旅游局`),
      field("房主姓名", `${item.name}运营管理有限公司`),
      field("房主联系电话", `0851-${36120000 + index}`),
      field("房主证件号码", `9152****${(320 + index).toString().padStart(4, "0")}`),
    ]),
    section("经营运营信息", [
      field("经营类型", item.name.includes("民宿") ? "精品民宿" : "酒店住宿"),
      field("房间名称", item.name.includes("民宿") ? "山景套房 / 亲子套房 / 庭院房" : "豪华双床房 / 商务大床房 / 观景套房", { full: true }),
      field("经营形式", index % 2 === 0 ? "直营网营" : "品牌加盟"),
      field("经营形式备注", "同时接入线上 OTA、团队接待和电话预订渠道", { full: true }),
      field("等级评定", item.level),
      field("客房数", `${88 + index * 4}间`),
      field("床位数", `${168 + index * 7}张`),
      field("从业人员数", `${32 + index * 2}人`),
      field("是否已接入公安旅馆治安系统", index % 5 === 0 ? "待接入" : "已接入"),
      field("在哪些 OTA 平台上线", ["携程", "美团", "飞猪", "同程"], { full: true }),
      field("其他平台名称", "我行商旅、贵客荟"),
      field("住宿单位门头照", "已上传门头照、前台照、消防通道照", { full: true }),
      field("普查人员", `普查员${index + 1}`),
      field("填报工作人员", `工作人员${index + 1}`),
    ]),
  ];
}

function buildMuseumSections(item, index) {
  return [
    section("博物馆基础字段", [
      field("博物馆名称", item.name),
      field("隶属层级", index % 5 === 0 ? "省级" : "市州级"),
      field("博物馆性质", index % 3 === 0 ? "国有综合类博物馆" : "专题类博物馆"),
      field("质量等级", index % 4 === 0 ? "国家一级博物馆" : "省级重点文化场馆"),
      field("题材类型", ["历史文化类", "民族民俗类", "革命纪念类", "综合展示类"][index % 4]),
      field("营业状态", "正常开放"),
      field("是否文保单位", index % 3 === 0 ? "是" : "否"),
      field("最晚入馆时间", "16:00"),
      field("每周开放天数", index % 6 === 0 ? "5天" : "6天"),
      field("省份", item.province),
      field("市州", item.city),
      field("区县", item.county),
      field("详细地址", item.address, { full: true }),
      field("经度", item.longitude),
      field("纬度", item.latitude),
      field("展厅面积", `${8600 + index * 430}平方米`),
      field("室外场所面积", `${2200 + index * 180}平方米`),
    ], { note: "文化场馆中的博物馆、纪念馆类资源采用该字段标准。" }),
    section("场馆介绍与服务", [
      field("官方介绍", `${item.name}承担贵州历史文化、民族文化或公共文化资源展示职能。`, { full: true }),
      field("详细介绍", `${item.name}已关联预约数据、展览活动数据、数字访问量和文化资源目录，可支撑数字文化馆、政府智能体和知识库应用。`, { full: true }),
    ]),
  ];
}

function buildCommonSections(item, spec, index) {
  const commonIntro = `${item.name}已纳入全省${item.category}资源目录管理，支撑资源查询、数据关联和业务协同。`;
  switch (spec.type) {
    case "street":
      return [
        section("街区基础信息", [
          field("街区名称", item.name),
          field("街区等级", item.level),
          field("营业状态", "正常开放"),
          field("街区类型", ["历史文化街区", "休闲消费街区", "夜游街区"][index % 3]),
          field("是否夜游", index % 2 === 0 ? "是" : "否"),
          field("核心业态", "餐饮、文创、非遗、演艺", { full: true }),
          field("省份", item.province),
          field("市州", item.city),
          field("区县", item.county),
          field("详细地址", item.address, { full: true }),
          field("经度", item.longitude),
          field("纬度", item.latitude),
        ]),
        section("关联情况", [
          field("简介", commonIntro, { full: true }),
          field("重点活动频次", `${6 + (index % 8)}场/月`),
          field("关联数据", item.linkedData, { full: true }),
        ]),
      ];
    case "resort":
      return [
        section("度假区基础信息", [
          field("度假区名称", item.name),
          field("创建级别", item.level),
          field("营业状态", "正常运营"),
          field("主导业态", "观光度假、康养住宿、山地运动", { full: true }),
          field("省份", item.province),
          field("市州", item.city),
          field("区县", item.county),
          field("详细地址", item.address, { full: true }),
          field("经度", item.longitude),
          field("纬度", item.latitude),
          field("核心住宿床位", `${2200 + index * 140}张`),
          field("综合评分", `${(4.4 + (index % 4) * 0.1).toFixed(1)} / 5`),
        ]),
        section("运营与服务", [
          field("简介", commonIntro, { full: true }),
          field("是否预约", index % 2 === 0 ? "是" : "否"),
          field("关联数据", item.linkedData, { full: true }),
        ]),
      ];
    case "village":
      return [
        section("乡村旅游点基础信息", [
          field("旅游点名称", item.name),
          field("资源等级", item.level),
          field("示范类型", ["重点旅游村", "民族文化村", "康养避暑点"][index % 3]),
          field("营业状态", "正常接待"),
          field("省份", item.province),
          field("市州", item.city),
          field("区县", item.county),
          field("详细地址", item.address, { full: true }),
          field("经度", item.longitude),
          field("纬度", item.latitude),
          field("特色资源", "民族文化、田园景观、民宿体验", { full: true }),
        ]),
        section("服务与带动", [
          field("简介", commonIntro, { full: true }),
          field("民宿数量", `${18 + index}家`),
          field("关联数据", item.linkedData, { full: true }),
        ]),
      ];
    case "agency":
      return [
        section("旅行社基础信息", [
          field("旅行社名称", item.name),
          field("机构等级", item.level),
          field("经营状态", "正常营业"),
          field("经营范围", "国内旅游、入境旅游、定制线路", { full: true }),
          field("省份", item.province),
          field("市州", item.city),
          field("区县", item.county),
          field("详细地址", item.address, { full: true }),
          field("统一社会信用代码", `9152010${index}MA6L****${index + 3}`),
          field("导游人数", `${22 + index}人`),
          field("门店数量", `${1 + (index % 4)}个`),
        ]),
        section("监管与信用", [
          field("简介", commonIntro, { full: true }),
          field("信用等级", ["A", "AA", "AAA"][index % 3]),
          field("关联数据", item.linkedData, { full: true }),
        ]),
      ];
    case "guide":
      return [
        section("导游基础信息", [
          field("姓名", item.name.replace("导游 ", "")),
          field("等级", item.level),
          field("执业状态", "正常执业"),
          field("导游证号", `GUIDE5201${(1000 + index).toString()}`),
          field("执业语种", ["中文", "中英双语", "中韩双语"][index % 3]),
          field("归属地区", `${item.city}${item.county}`),
          field("联系电话", maskedPhone(index + 60)),
          field("擅长线路", "山地观光、民族文化、红色研学", { full: true }),
          field("综合评分", `${(4.6 + (index % 3) * 0.1).toFixed(1)} / 5`),
        ]),
        section("监管与服务", [
          field("简介", commonIntro, { full: true }),
          field("信用等级", ["良好", "优秀", "重点关注"][index % 3]),
          field("关联数据", item.linkedData, { full: true }),
        ]),
      ];
    case "event":
      return [
        section("活动基础信息", [
          field("活动名称", item.name),
          field("活动级别", item.level),
          field("活动类型", ["体育赛事", "节庆活动", "文艺演出", "推广活动"][index % 4]),
          field("举办时间", `2026-${((index % 8) + 1).toString().padStart(2, "0")}-${((index % 18) + 10).toString().padStart(2, "0")}`),
          field("举办地点", item.address, { full: true }),
          field("主办单位", `${item.city}文化和旅游局`),
          field("是否收费", item.chargeMode === "免费" ? "否" : "是"),
          field("预计客流", `${1.8 + index * 0.12}万人次`),
        ]),
        section("传播与带动", [
          field("简介", commonIntro, { full: true }),
          field("传播热度指数", `${78 + index}`),
          field("关联数据", item.linkedData, { full: true }),
        ]),
      ];
    case "business":
      const businessTypeMap = {
        餐饮主体: "餐饮服务",
        购物主体: "购物服务",
        旅游服务主体: "旅游服务",
      };
      return [
        section("经营主体基础信息", [
          field("单位名称", item.name),
          field("主体类型", businessTypeMap[item.category] ?? "旅游服务"),
          field("经营状态", "正常营业"),
          field("所属地区", `${item.city}${item.county}`),
          field("详细地址", item.address, { full: true }),
          field("经度", item.longitude),
          field("纬度", item.latitude),
          field("统一社会信用代码", `9152****${(6000 + index).toString()}`),
          field("信用等级", ["A", "AA", "AAA"][index % 3]),
        ]),
        section("监管与消费", [
          field("简介", commonIntro, { full: true }),
          field("重点消费场景", "餐饮、文创、游客服务、活动消费", { full: true }),
          field("关联数据", item.linkedData, { full: true }),
        ]),
      ];
    default:
      return [
        section("基础信息", [
          field("资源名称", item.name),
          field("资源类型", item.category),
          field("等级", item.level),
          field("省份", item.province),
          field("市州", item.city),
          field("区县", item.county),
          field("详细地址", item.address, { full: true }),
          field("经度", item.longitude),
          field("纬度", item.latitude),
        ]),
        section("资源介绍", [field("简介", commonIntro, { full: true })]),
      ];
  }
}

const categoryConfigs = {
  景区: {
    type: "scenic",
    names: scenicNames,
    levelPool: ["5A景区", "5A景区", "4A景区", "4A景区"],
    openingPool: ["07:30-18:00", "08:00-17:30", "08:30-18:30"],
    chargePool: ["收费", "收费", "部分收费"],
    linkedDataPool: ["闸机实时客流", "门票订单", "停车场监测", "LBS热度", "视频监控", "舆情快报"],
    intro: (name) => `${name}是贵州重点景区资源，已纳入重点景区客流监测和应急指挥联动范围。`,
    sectionBuilder: buildScenicSections,
  },
  街区: {
    type: "street",
    names: streetNames,
    levelPool: ["省级旅游休闲街区", "国家级旅游休闲街区", "重点文旅消费街区"],
    openingPool: ["10:00-22:00", "全天开放", "14:00-23:00"],
    chargePool: ["免费", "部分收费", "免费"],
    linkedDataPool: ["街区活动数据", "周边消费热度", "停车场数据", "互联网传播数据"],
    intro: (name) => `${name}是兼具游览、消费和夜间活动的重点街区资源。`,
    sectionBuilder: buildCommonSections,
  },
  度假区: {
    type: "resort",
    names: resortNames,
    levelPool: ["国家级旅游度假区", "省级旅游度假区", "重点康养度假区"],
    openingPool: ["全天运营", "08:00-20:00", "09:00-21:00"],
    chargePool: ["收费", "收费", "部分收费"],
    linkedDataPool: ["住宿运行数据", "景区基础档案", "活动计划", "游客画像", "消费数据"],
    intro: (name) => `${name}聚焦山地度假、康养休闲和住宿运营，是综合型度假资源。`,
    sectionBuilder: buildCommonSections,
  },
  乡村旅游点: {
    type: "village",
    names: villageNames,
    levelPool: ["4A旅游点", "重点旅游村", "示范民宿聚集点"],
    openingPool: ["全天开放", "08:30-18:00", "09:00-20:00"],
    chargePool: ["收费", "免费", "部分收费"],
    linkedDataPool: ["民宿档案", "LBS客流", "游客画像", "活动传播数据"],
    intro: (name) => `${name}是兼具民族文化展示、乡村休闲和民宿经营场景的代表性乡村旅游点。`,
    sectionBuilder: buildCommonSections,
  },
  文化场馆: {
    type: "culture",
    names: cultureNames,
    levelPool: ["省级重点文化场馆", "国家一级博物馆", "市州级重点场馆"],
    openingPool: ["09:00-17:00（周一闭馆）", "09:30-17:30", "09:00-16:30"],
    chargePool: ["免费", "免费", "部分收费"],
    linkedDataPool: ["馆藏资源目录", "展览活动数据", "预约数据", "数字访问量"],
    intro: (name) => `${name}承担公共文化服务、文化传播和数字化展示的重要职能。`,
    sectionBuilder: buildMuseumSections,
  },
  酒店民宿: {
    type: "hotel",
    names: hotelNames,
    levelPool: ["四星级酒店", "精品民宿", "三星级酒店", "高端度假酒店"],
    openingPool: ["24小时营业", "全天接待", "全天运营"],
    chargePool: ["收费", "收费", "收费"],
    linkedDataPool: ["住业普查底账", "酒店订单", "入住统计", "PMS接入状态", "公安旅馆治安系统"],
    intro: (name) => `${name}是已纳入住宿资源底账和经营运行监测范围的重点住宿经营主体。`,
    sectionBuilder: buildHotelSections,
  },
  旅行社: {
    type: "agency",
    names: agencyNames,
    levelPool: ["AAAAA级旅行社创建单位", "重点旅行社", "省级示范旅行社"],
    openingPool: ["09:00-18:00", "08:30-18:30", "09:30-18:00"],
    chargePool: ["收费", "收费", "收费"],
    linkedDataPool: ["旅行社档案", "团队游客数据", "信用监管信息", "合同台账"],
    intro: (name) => `${name}承担线路组织、团队接待和旅游服务保障，是重点涉旅服务主体。`,
    sectionBuilder: buildCommonSections,
  },
  导游: {
    type: "guide",
    names: guideNames,
    levelPool: ["高级导游", "中级导游", "金牌导游"],
    openingPool: ["按排团安排", "全天待命", "按预约安排"],
    chargePool: ["收费", "收费", "收费"],
    linkedDataPool: ["导游执业档案", "信用评级", "游客评价", "投诉处置记录"],
    intro: (name) => `${name}已纳入导游执业监管、信用评价和服务质量跟踪体系。`,
    sectionBuilder: buildCommonSections,
  },
  餐饮主体: {
    type: "business",
    names: cateringNames,
    levelPool: ["重点餐饮主体", "特色餐饮主体", "重点消费主体"],
    openingPool: ["10:00-22:00", "09:00-21:30", "全天营业"],
    chargePool: ["收费", "收费", "收费"],
    linkedDataPool: ["经营主体档案", "行政监管信息", "消费热度数据", "信用评价"],
    intro: (name) => `${name}与景区、街区和重点活动消费场景联动，纳入经营监管与消费监测。`,
    sectionBuilder: buildCommonSections,
  },
  购物主体: {
    type: "business",
    names: shoppingNames,
    levelPool: ["重点购物主体", "旅游商品主体", "文创经营主体"],
    openingPool: ["10:00-22:00", "09:00-21:30", "全天营业"],
    chargePool: ["收费", "收费", "收费"],
    linkedDataPool: ["经营主体档案", "行政监管信息", "消费热度数据", "信用评价"],
    intro: (name) => `${name}承担旅游商品、文创商品和伴手礼经营服务，是重点旅游购物主体。`,
    sectionBuilder: buildCommonSections,
  },
  旅游服务主体: {
    type: "business",
    names: tourismServiceNames,
    levelPool: ["重点旅游服务主体", "配套服务主体", "游客服务主体"],
    openingPool: ["10:00-22:00", "09:00-21:30", "全天营业"],
    chargePool: ["收费", "收费", "收费"],
    linkedDataPool: ["经营主体档案", "行政监管信息", "消费热度数据", "信用评价"],
    intro: (name) => `${name}承担游客集散、骑行、装备、综合服务等配套保障，是重点旅游服务主体。`,
    sectionBuilder: buildCommonSections,
  },
};

function buildAliases(name, category, index) {
  if (category === "景区") return [name.replace("旅游区", "景区"), `${name}风景区`];
  if (category === "酒店民宿") return [name.replace("大酒店", "酒店"), `${name}住宿点`];
  if (category === "文化场馆") return [name.replace("贵州省", "省"), `${name}资源点`];
  if (category === "街区") return [name.replace("街区", "片区"), `${name}文旅区`];
  return [`${name}${index % 2 === 0 ? "资源点" : "基础档案"}`];
}

function createResourceRecords(category, config) {
  return config.names.map((name, index) => {
    const region = regionAt(index);
    const item = {
      id: `${config.type}-${index + 1}`,
      name,
      category,
      aliases: buildAliases(name, category, index),
      level: config.levelPool[index % config.levelPool.length],
      province: "贵州省",
      city: region.city,
      county: region.county,
      address: `贵州省${region.city}${region.county}${name.includes("导游") ? "注册归属地" : name.includes("活动") ? "主会场" : "核心运营点"}`,
      longitude: coord(104.65, index, 0.18),
      latitude: coord(25.08, index, 0.11),
      openingHours: config.openingPool[index % config.openingPool.length],
      chargeMode: config.chargePool[index % config.chargePool.length],
      owner: `${region.city}文化和旅游局`,
      intro: config.intro(name),
      linkedData: config.linkedDataPool,
    };
    item.detailSections = config.sectionBuilder(item, config, index);
    return item;
  });
}

const resources = Object.entries(categoryConfigs).flatMap(([category, config]) => createResourceRecords(category, config));

const businessSystems = [
  {
    name: "旅游资源基础数据",
    type: "资源基础",
    module: "资源管理",
    content: "景区、街区、旅游度假区、特色打卡点等名称、位置、经纬度、介绍、开放时间、门票价格。",
    purpose: "资源底图、智能推荐、导览问答",
    status: "已接入",
    priority: "高",
    ownerDept: "资源开发处",
  },
  {
    name: "贵州省乡村旅游综合管理平台",
    type: "业务系统",
    module: "业务系统目录",
    content: "重点旅游村镇、示范民宿、农家乐的申报、审核、复核。",
    purpose: "乡村旅游资源管理、示范点监管",
    status: "已接入",
    priority: "高",
    ownerDept: "乡村旅游处",
    loginUrl: "http://61.243.11.150:8081",
  },
  {
    name: "贵州省文化和旅游市场监督执法平台-文化方向",
    type: "业务系统",
    module: "业务系统目录",
    content: "文娱场所管理、视频监控、营业/停业/变更审核、法律法规、执法。",
    purpose: "文化主体监管、执法治理",
    status: "已接入",
    priority: "高",
    ownerDept: "文化市场综合执法监督处",
    loginUrl: "https://www.gz12318.cn/",
  },
  {
    name: "贵州省文化和旅游市场监督执法平台-旅游方向",
    type: "业务系统",
    module: "业务系统目录",
    content: "民宿、导游、旅行社、景区景点、购物店、酒店住宿、餐饮服务、保单管理、合同列表、舆情管理、执法队伍。",
    purpose: "涉旅主体监管、市场治理",
    status: "已接入",
    priority: "高",
    ownerDept: "市场管理处",
    loginUrl: "https://ct.wljdzf.cn/backend/#/page/612",
  },
  {
    name: "贵州省文旅信用监管平台",
    type: "业务系统",
    module: "业务系统目录",
    content: "旅行社、导游等涉旅主体信用监管。",
    purpose: "信用画像、联合监管",
    status: "已接入",
    priority: "高",
    ownerDept: "市场管理处",
    loginUrl: "http://61.243.11.121:8088",
  },
  {
    name: "“行游贵州”旅游安全预警提示系统",
    type: "业务系统",
    module: "业务系统目录",
    content: "天气、交通、人流等安全监测与预警、紧急救援服务信息。",
    purpose: "安全预警、风险识别、应急联动",
    status: "已接入",
    priority: "高",
    ownerDept: "安全应急处",
  },
  {
    name: "贵州省旅游规划监管平台",
    type: "业务系统",
    module: "业务系统目录",
    content: "各级旅游规划成果、规划执行与监管信息。",
    purpose: "规划监管、资源统筹",
    status: "已接入",
    priority: "中",
    ownerDept: "资源开发处",
  },
  {
    name: "贵州全域旅游数字化服务平台",
    type: "业务系统",
    module: "业务系统目录",
    content: "游客数量、收入情况、景区运营状况等全域旅游数据。",
    purpose: "运行监测、领导决策",
    status: "已接入",
    priority: "高",
    ownerDept: "产业发展处",
  },
  {
    name: "贵州数字文化馆",
    type: "业务系统",
    module: "业务系统目录",
    content: "传统文化、非遗、多媒体文化展示数据。",
    purpose: "文化资源展示、知识库建设",
    status: "已接入",
    priority: "中",
    ownerDept: "公共服务处",
  },
  {
    name: "活动管理系统",
    type: "业务系统",
    module: "业务系统目录",
    content: "贵阳市街道活动基础数据，后续可接入全省旅游文化活动，并纳入互联网传播数据。",
    purpose: "活动管理、传播评估、经济影响分析",
    status: "已接入",
    priority: "高",
    ownerDept: "宣传推广处",
  },
  {
    name: "住业普查系统",
    type: "业务系统",
    module: "业务系统目录",
    content: "全省酒店、民宿等住宿资源单底数，含工商数据和基础信息。",
    purpose: "住宿资源底账、住宿主数据",
    status: "已接入",
    priority: "高",
    ownerDept: "市场管理处",
  },
  {
    name: "PMS（待接入数据）",
    type: "在建系统",
    module: "数据接入",
    content: "游客入住、离店、间夜、房态、价格、渠道等经营数据。",
    purpose: "过夜游客分析、经营监测",
    status: "待接入",
    priority: "高",
    ownerDept: "市场管理处",
  },
  {
    name: "酒店智能体",
    type: "智能应用",
    module: "数据资产中心",
    content: "酒店商家入住信息、酒店知识库数据。",
    purpose: "酒店问答、经营助手、服务推荐",
    status: "已运行",
    priority: "中",
    ownerDept: "信息中心",
  },
  {
    name: "黄小西智能体",
    type: "智能应用",
    module: "数据资产中心",
    content: "景区基础数据、活动数据、知识库问答、行程规划及酒店/车票/机票下单能力。",
    purpose: "游客服务、伴游、智能规划",
    status: "已运行",
    priority: "高",
    ownerDept: "信息中心",
  },
  {
    name: "我行商旅",
    type: "外部数据",
    module: "业务系统目录",
    content: "酒店基础信息、酒店订单信息。",
    purpose: "订单分析、酒店供给补充",
    status: "已接入",
    priority: "中",
    ownerDept: "市场管理处",
  },
  {
    name: "LBS与消费数据",
    type: "外部数据",
    module: "数据资产中心",
    content: "百度客流、电信客流、高德消费数据，覆盖省、市州、区县和重点景区。",
    purpose: "客流监测、消费分析、区域对比",
    status: "已接入",
    priority: "高",
    ownerDept: "数据办",
  },
  {
    name: "贵客荟",
    type: "外部数据",
    module: "数据接入",
    content: "景区预约、酒店PMS、文旅监管数据，覆盖酒店、景区、民宿、黔货、旅游线路。",
    purpose: "预约订单汇聚、供给与交易联动",
    status: "已接入",
    priority: "高",
    ownerDept: "市场管理处",
  },
  {
    name: "气象数据",
    type: "外部数据",
    module: "数据资产中心",
    content: "气象预报、气象预警。",
    purpose: "气象风险识别、主动预警",
    status: "已接入",
    priority: "高",
    ownerDept: "安全应急处",
  },
  {
    name: "应急指挥子系统（在建）",
    type: "在建系统",
    module: "业务系统目录",
    content: "景区视频监控、事件告警、通信联络、调度指令，建设监测、预警、通信、调度一体化能力。",
    purpose: "视频会商、事件处置、应急指挥",
    status: "建设中",
    priority: "高",
    ownerDept: "安全应急处",
  },
];

const dataAccessCatalog = [
  {
    name: "旅游资源基础数据",
    dataType: "旅游资源数据",
    sourceChannel: "资源目录与业务填报",
    accessDate: "2025-03-01",
    refresh: "按变更实时更新",
    status: "已接入",
    originSystem: "旅游资源基础数据",
  },
  {
    name: "PMS入住经营数据",
    dataType: "住宿业数据",
    sourceChannel: "酒店 PMS",
    accessDate: "2026-07-01",
    refresh: "实时",
    status: "建设中",
    originSystem: "PMS（待接入数据）",
  },
  {
    name: "酒店智能体知识库数据",
    dataType: "知识数据",
    sourceChannel: "酒店智能体",
    accessDate: "2026-02-01",
    refresh: "每周",
    status: "已接入",
    originSystem: "酒店智能体",
  },
  {
    name: "黄小西景区知识与活动数据",
    dataType: "知识数据",
    sourceChannel: "黄小西智能体",
    accessDate: "2026-02-15",
    refresh: "每周",
    status: "已接入",
    originSystem: "黄小西智能体",
  },
  {
    name: "我行商旅酒店订单数据",
    dataType: "订单数据",
    sourceChannel: "我行商旅",
    accessDate: "2025-06-01",
    refresh: "每日",
    status: "已接入",
    originSystem: "我行商旅",
  },
  {
    name: "百度全省实时客流",
    dataType: "LBS数据",
    sourceChannel: "百度",
    accessDate: "2026-01-01",
    refresh: "5分钟",
    status: "已接入",
    originSystem: "LBS与消费数据",
  },
  {
    name: "电信客流监测数据",
    dataType: "LBS数据",
    sourceChannel: "中国电信",
    accessDate: "2026-01-01",
    refresh: "每小时",
    status: "已接入",
    originSystem: "LBS与消费数据",
  },
  {
    name: "高德文旅消费热度数据",
    dataType: "消费数据",
    sourceChannel: "高德",
    accessDate: "2026-01-01",
    refresh: "每日",
    status: "已接入",
    originSystem: "LBS与消费数据",
  },
  {
    name: "贵客荟预约与交易数据",
    dataType: "平台数据",
    sourceChannel: "贵客荟",
    accessDate: "2025-08-01",
    refresh: "每日",
    status: "已接入",
    originSystem: "贵客荟",
  },
  {
    name: "气象预测数据",
    dataType: "气象数据",
    sourceChannel: "中国天气网",
    accessDate: "2025-05-01",
    refresh: "每小时",
    status: "已接入",
    originSystem: "气象数据",
  },
  {
    name: "气象告警数据",
    dataType: "气象数据",
    sourceChannel: "中国天气网",
    accessDate: "2025-05-01",
    refresh: "5分钟",
    status: "已接入",
    originSystem: "气象数据",
  },
  {
    name: "景区视频监控流",
    dataType: "视频数据",
    sourceChannel: "应急指挥子系统",
    accessDate: "2026-09-01",
    refresh: "实时",
    status: "建设中",
    originSystem: "应急指挥子系统（在建）",
  },
];

function buildAccessDailyRecords(item, index) {
  const tablePrefix = item.dataType.includes("住宿")
    ? "ods_hotel"
    : item.dataType.includes("知识")
      ? "dim_knowledge"
      : item.dataType.includes("LBS")
        ? "dwd_lbs"
        : item.dataType.includes("消费")
          ? "ads_consume"
          : item.dataType.includes("气象")
            ? "ods_weather"
            : item.dataType.includes("视频")
              ? "ods_video"
              : item.dataType.includes("平台")
                ? "ods_platform"
                : "mdm_resource";
  const tableNames = [
    `${tablePrefix}_daily`,
    `${tablePrefix}_detail`,
    `${tablePrefix}_summary`,
  ];

  return tableNames.map((tableName, recordIndex) => {
    const day = 11 - recordIndex;
    const hour = 1 + ((index + recordIndex) % 4);
    const isBuilding = item.status !== "已接入" && recordIndex === 0;
    const startTime = `2026-07-${String(day).padStart(2, "0")} ${String(hour).padStart(2, "0")}:10`;
    const endTime = isBuilding ? "执行中" : `2026-07-${String(day).padStart(2, "0")} ${String(hour).padStart(2, "0")}:${42 + recordIndex * 3}`;

    return {
      tableName,
      updateDate: `2026-07-${String(day).padStart(2, "0")}`,
      updateTime: isBuilding ? "执行中" : endTime,
      updatedRows: isBuilding ? "同步中" : `${(8.6 + index * 1.7 + recordIndex * 0.9).toFixed(1)}万行`,
      totalRows: `${(128 + index * 36 + recordIndex * 12).toLocaleString("zh-CN")}万行`,
      taskStartTime: startTime,
      taskEndTime: endTime,
      taskStatus: isBuilding ? "进行中" : "成功",
    };
  });
}

const dataAssets = [
  {
    name: "旅游资源主数据",
    type: "主数据资产",
    source: "旅游资源基础数据、乡村旅游综合管理平台",
    refresh: "按变更实时更新",
    serviceScenes: "资源目录、智能推荐、导览问答",
    status: "已沉淀",
  },
  {
    name: "住宿资源主数据",
    type: "主数据资产",
    source: "住业普查系统、我行商旅",
    refresh: "每日",
    serviceScenes: "住宿目录、酒店智能体、经营监管",
    status: "已沉淀",
  },
  {
    name: "涉旅主体主数据",
    type: "主数据资产",
    source: "文化和旅游市场监督执法平台-旅游方向、文旅信用监管平台",
    refresh: "每日",
    serviceScenes: "旅行社、导游、涉旅经营主体统一底账与监管联动",
    status: "已沉淀",
  },
  {
    name: "文化资源主数据",
    type: "主数据资产",
    source: "贵州数字文化馆、文化场馆资源目录",
    refresh: "按变更实时更新",
    serviceScenes: "文化场馆目录、文化展示、知识库建设",
    status: "建设中",
  },
  {
    name: "景区客流主题数据集",
    type: "主题数据资产",
    source: "景区闸机、票务、LBS与消费数据",
    refresh: "5分钟",
    serviceScenes: "驾驶舱、节假日调度、智能问数",
    status: "建设中",
  },
  {
    name: "住宿经营运行主题集",
    type: "主题数据资产",
    source: "PMS、住业普查系统、我行商旅",
    refresh: "每日",
    serviceScenes: "过夜游客分析、酒店经营监测",
    status: "建设中",
  },
  {
    name: "文化市场监管主题集",
    type: "主题数据资产",
    source: "文化和旅游市场监督执法平台-文化方向",
    refresh: "每日",
    serviceScenes: "文化主体监管、执法治理",
    status: "已沉淀",
  },
  {
    name: "旅游市场监管主题集",
    type: "主题数据资产",
    source: "文化和旅游市场监督执法平台-旅游方向",
    refresh: "每日",
    serviceScenes: "涉旅主体监管、投诉治理、舆情联动",
    status: "已沉淀",
  },
  {
    name: "文旅信用主题集",
    type: "主题数据资产",
    source: "文旅信用监管平台",
    refresh: "每日",
    serviceScenes: "信用画像、联合监管、风险预警",
    status: "已沉淀",
  },
  {
    name: "安全预警与事件处置主题集",
    type: "主题数据资产",
    source: "行游贵州旅游安全预警提示系统、应急指挥子系统",
    refresh: "实时",
    serviceScenes: "风险识别、视频会商、事件调度",
    status: "建设中",
  },
  {
    name: "活动传播评估主题集",
    type: "主题数据资产",
    source: "活动管理系统、互联网公开数据",
    refresh: "每日",
    serviceScenes: "活动复盘、传播评估、经济影响分析",
    status: "已沉淀",
  },
  {
    name: "全域运行监测主题集",
    type: "主题数据资产",
    source: "贵州全域旅游数字化服务平台、LBS与消费数据",
    refresh: "每日",
    serviceScenes: "领导决策、运行监测、综合研判",
    status: "已沉淀",
  },
  {
    name: "LBS客流主题集",
    type: "标签资产",
    source: "百度客流、电信客流",
    refresh: "每日",
    serviceScenes: "客源地分析、区域客流对比",
    status: "已沉淀",
  },
  {
    name: "文旅消费热度主题集",
    type: "标签资产",
    source: "高德消费、OTA订单、贵客荟",
    refresh: "每日",
    serviceScenes: "消费分析、营销评估、活动带动",
    status: "已沉淀",
  },
  {
    name: "游客画像标签库",
    type: "标签资产",
    source: "LBS与消费数据、预约订单、住宿数据",
    refresh: "每日",
    serviceScenes: "客源地分析、精准营销、智能推荐",
    status: "建设中",
  },
  {
    name: "涉旅主体信用标签库",
    type: "标签资产",
    source: "文旅信用监管平台、市场监督执法平台",
    refresh: "每日",
    serviceScenes: "信用画像、风险分层、联合监管",
    status: "已沉淀",
  },
  {
    name: "酒店知识库",
    type: "知识资产",
    source: "酒店智能体、住宿主数据",
    refresh: "每周",
    serviceScenes: "酒店问答、商家服务助手",
    status: "已沉淀",
  },
  {
    name: "景区知识库",
    type: "知识资产",
    source: "黄小西智能体、旅游资源基础数据、活动管理系统",
    refresh: "每周",
    serviceScenes: "行程规划、伴游问答、景区服务",
    status: "已沉淀",
  },
  {
    name: "数字文化资源知识库",
    type: "知识资产",
    source: "贵州数字文化馆、文化场馆资源目录",
    refresh: "每周",
    serviceScenes: "文化问答、知识检索、数字展示",
    status: "建设中",
  },
  {
    name: "视频监控专题资源",
    type: "知识资产",
    source: "应急指挥子系统",
    refresh: "实时",
    serviceScenes: "视频会商、事件核验、景区监测",
    status: "建设中",
  },
  {
    name: "气象风险专题集",
    type: "指标资产",
    source: "气象数据、行游贵州旅游安全预警提示系统",
    refresh: "实时",
    serviceScenes: "主动预警、出行提示、风险研判",
    status: "已沉淀",
  },
  {
    name: "全域运行监测专题集",
    type: "指标资产",
    source: "贵州全域旅游数字化服务平台、LBS与消费数据",
    refresh: "每日",
    serviceScenes: "领导决策、运行监测、综合研判",
    status: "已沉淀",
  },
  {
    name: "统一指标口径库",
    type: "指标资产",
    source: "指标中心、驾驶舱重构结果",
    refresh: "按口径调整同步更新",
    serviceScenes: "驾驶舱、问数、报告生成、共享交换",
    status: "建设中",
  },
  {
    name: "旅游资源主数据查询服务",
    type: "服务资产",
    source: "资源主数据服务层",
    refresh: "实时",
    serviceScenes: "资源目录、外部系统调用、共享交换",
    status: "已沉淀",
  },
  {
    name: "景区客流专题服务",
    type: "服务资产",
    source: "景区客流主题数据集、统一指标服务",
    refresh: "5分钟",
    serviceScenes: "驾驶舱、节假日调度、领导专题",
    status: "已沉淀",
  },
  {
    name: "应急视频会商服务",
    type: "服务资产",
    source: "应急指挥子系统、视频监控专题资源",
    refresh: "实时",
    serviceScenes: "视频会商、事件调度、风险核验",
    status: "建设中",
  },
  {
    name: "智能问数与知识检索服务",
    type: "服务资产",
    source: "政府智能体、景区知识库、酒店知识库",
    refresh: "实时",
    serviceScenes: "智能问数、智能问策、知识问答",
    status: "已沉淀",
  },
];

const metrics = [
  {
    name: "发布客流",
    domain: "客流监测",
    description: "重点景区统一对外发布的客流指标。",
    rule: "优先采用闸机或票务核销人数，缺失时以景区上报补位，LBS用于趋势校验。",
    source: "闸机客流 / 票务核销 / 景区上报 / LBS趋势校验",
    refresh: "5分钟",
    owner: "数据办",
    scenes: "驾驶舱、节假日调度、智能问数",
  },
  {
    name: "实时在园人数",
    domain: "客流监测",
    description: "反映重点景区当前在园游客承载情况。",
    rule: "按入园人数减去出园人数形成在园人数，5分钟滚动更新。",
    source: "闸机实时客流",
    refresh: "5分钟",
    owner: "资源开发处",
    scenes: "景区调度、承载预警、应急指挥",
  },
  {
    name: "日累计入园人数",
    domain: "客流监测",
    description: "反映当日景区累计接待规模。",
    rule: "按闸机入园累计或票务核销累计形成日累计指标。",
    source: "闸机实时客流 / 门票核销",
    refresh: "5分钟",
    owner: "资源开发处",
    scenes: "假日监测、日报通报、客流复盘",
  },
  {
    name: "住宿入住率",
    domain: "住宿运行",
    description: "反映辖区住宿经营主体有效房量使用情况。",
    rule: "按有效房量与已入住间夜测算，剔除停业、改造和不可售房源。",
    source: "住宿业PMS / 住业普查底账 / 订单平台",
    refresh: "每日",
    owner: "产业发展处",
    scenes: "住宿监测、节假日调度、酒店智能体",
  },
  {
    name: "平均房价",
    domain: "住宿运行",
    description: "反映住宿市场价格水平与波动情况。",
    rule: "按已售间夜订单金额除以已售间夜数计算。",
    source: "PMS / OTA订单 / 我行商旅",
    refresh: "每日",
    owner: "产业发展处",
    scenes: "市场研判、价格波动分析",
  },
  {
    name: "过夜游客量",
    domain: "住宿运行",
    description: "反映全省或辖区过夜游客规模。",
    rule: "以有效入住旅客人数为主，结合住宿订单和入住记录去重汇总。",
    source: "PMS / 住业普查系统",
    refresh: "每日",
    owner: "数据办",
    scenes: "住宿运行、游客结构分析、报告生成",
  },
  {
    name: "消费热度指数",
    domain: "市场运行",
    description: "综合反映旅游消费活跃度与带动效果。",
    rule: "按消费笔数、客单价、交易活跃度和节假日权重形成综合指数。",
    source: "高德消费 / OTA订单 / 互联网公开数据",
    refresh: "每日",
    owner: "产业发展处",
    scenes: "产业分析、营销评估、活动复盘",
  },
  {
    name: "游客来源占比",
    domain: "游客画像",
    description: "反映省内、省外、市州和区县客源结构。",
    rule: "基于 LBS、订单和入住数据形成来源地分布，占比按去重游客计算。",
    source: "LBS客流 / 订单 / 住宿数据",
    refresh: "每日",
    owner: "宣传推广处",
    scenes: "客源地分析、精准营销",
  },
  {
    name: "活动传播指数",
    domain: "活动传播",
    description: "衡量重点活动在互联网传播端的热度和影响力。",
    rule: "综合阅读量、互动量、话题声量、平台覆盖度形成指数。",
    source: "活动管理系统 / 互联网公开数据",
    refresh: "每日",
    owner: "宣传推广处",
    scenes: "活动复盘、传播评估、领导汇报",
  },
  {
    name: "活动风险等级",
    domain: "安全应急",
    description: "对重点活动及景区突发风险进行分级预警。",
    rule: "综合天气、视频监控、客流阈值、停车承载和事件上报情况形成风险等级。",
    source: "气象 / 视频监控 / 客流 / 停车 / 事件上报",
    refresh: "实时",
    owner: "市场管理处",
    scenes: "应急指挥、会商调度、主动预警",
  },
  {
    name: "视频在线率",
    domain: "安全应急",
    description: "反映景区视频监控在线接入和可用情况。",
    rule: "按在线摄像头数量除以纳管摄像头总数计算。",
    source: "应急指挥子系统",
    refresh: "实时",
    owner: "信息中心",
    scenes: "视频监测、系统运维、会商保障",
  },
  {
    name: "事件处置闭环率",
    domain: "安全应急",
    description: "反映应急事件从发现到办结的闭环处置情况。",
    rule: "按已办结事件数除以事件总数计算，逾期事件单独标识。",
    source: "应急指挥子系统 / 事件上报系统",
    refresh: "实时",
    owner: "市场管理处",
    scenes: "事件调度、督办通报、考核评价",
  },
];

const serviceCatalog = [
  {
    name: "旅游资源主数据查询 API",
    type: "API",
    provider: "数据底座服务层",
    target: "厅机关 / 市州区县 / 景区",
    auth: "按角色授权 + Token 调用",
    refresh: "实时",
    status: "已开通",
    detail: "提供景区、街区、度假区、文化场馆、酒店民宿等统一主数据查询能力。",
  },
  {
    name: "景区客流专题数据集",
    type: "专题数据集",
    provider: "客流主题资产库",
    target: "驾驶舱 / 领导专题 / 节假日调度",
    auth: "按专题审批",
    refresh: "5分钟",
    status: "已开通",
    detail: "按景区、时段、区域提供发布客流、在园人数和客流趋势数据。",
  },
  {
    name: "住宿运行专题数据集",
    type: "专题数据集",
    provider: "住宿运行资产库",
    target: "产业发展处 / 市州文旅单位",
    auth: "按专题审批",
    refresh: "每日",
    status: "建设中",
    detail: "覆盖入住率、平均房价、过夜游客量和有效房量等指标。",
  },
  {
    name: "文旅消费热度服务",
    type: "指标服务",
    provider: "消费主题资产库",
    target: "市场分析 / 活动评估 / 智能问数",
    auth: "按角色授权",
    refresh: "每日",
    status: "已开通",
    detail: "提供消费热度指数、消费笔数、客单价和节假日波动分析能力。",
  },
  {
    name: "游客画像标签服务",
    type: "指标服务",
    provider: "标签画像库",
    target: "宣传推广处 / 智能推荐应用",
    auth: "字段脱敏 + 审批授权",
    refresh: "每日",
    status: "建设中",
    detail: "支持省内外客源地占比、年龄分层、团队散客等游客画像查询。",
  },
  {
    name: "活动传播评估接口",
    type: "API",
    provider: "活动传播主题集",
    target: "活动管理系统 / 政府智能体",
    auth: "按接口申请",
    refresh: "每日",
    status: "已开通",
    detail: "提供活动传播指数、话题声量和平台覆盖度等指标服务。",
  },
  {
    name: "旅游安全预警推送接口",
    type: "API",
    provider: "安全预警主题集",
    target: "驾驶舱 / 应急指挥 / 行游贵州系统",
    auth: "系统间互信",
    refresh: "实时",
    status: "已开通",
    detail: "支持天气风险、客流超阈值、停车告警等事件主动推送。",
  },
  {
    name: "景区视频会商流服务",
    type: "视频服务",
    provider: "应急指挥子系统",
    target: "省厅值班室 / 市州文旅单位",
    auth: "专网授权",
    refresh: "实时",
    status: "建设中",
    detail: "提供景区重点点位视频流接入、在线查看和会商调用能力。",
  },
  {
    name: "统一指标查询服务",
    type: "指标服务",
    provider: "指标中心",
    target: "全系统目录模块",
    auth: "按角色授权",
    refresh: "实时",
    status: "已开通",
    detail: "将指标定义、口径、来源、责任单位和适用场景统一组织成查询服务。",
  },
  {
    name: "智能问数服务",
    type: "智能服务",
    provider: "政府智能体",
    target: "厅机关 / 处室业务用户",
    auth: "账号登录 + 问数权限",
    refresh: "实时",
    status: "已开通",
    detail: "围绕统一指标和主题资产提供自然语言问数能力。",
  },
  {
    name: "酒店知识库检索服务",
    type: "智能服务",
    provider: "酒店智能体知识库",
    target: "酒店民宿主体 / 运营人员",
    auth: "本单位授权",
    refresh: "每周",
    status: "已开通",
    detail: "提供酒店基础信息、入住政策、经营知识和服务流程的智能检索能力。",
  },
  {
    name: "景区伴游知识服务",
    type: "智能服务",
    provider: "黄小西智能体知识库",
    target: "游客服务应用 / 景区单位",
    auth: "应用级授权",
    refresh: "每周",
    status: "已开通",
    detail: "提供景区问答、行程规划、活动推荐和多场景伴游服务能力。",
  },
];

const queryThemeCatalog = [
  {
    id: "resource",
    label: "资源底账",
    description: "面向景区、酒店民宿、文化场馆和涉旅主体开展主数据与资源明细查询。",
    metrics: ["resource_total", "scenic_count", "hotel_count", "cultural_venue_count", "travel_subject_count"],
  },
  {
    id: "flow",
    label: "客流监测",
    description: "围绕 LBS 客流、闸机客流和在园承载等指标开展多维趋势与分布查询。",
    metrics: ["lbs_flow", "gate_flow", "park_visitors", "visitor_origin_ratio"],
  },
  {
    id: "operation",
    label: "景区运营",
    description: "围绕票务、订单、停车和景区经营运行情况开展多维查询与专题分析。",
    metrics: ["ticket_orders", "ticket_revenue", "parking_capacity", "parking_vacancy_rate"],
  },
  {
    id: "stay",
    label: "住宿运行",
    description: "围绕酒店民宿入住、房价、过夜游客和接入情况开展多维查询。",
    metrics: ["stay_visitors", "occupancy_rate", "avg_room_rate", "pms_connected_hotels"],
  },
  {
    id: "consume",
    label: "消费运行",
    description: "围绕文旅消费热度、消费笔数、客单价和活动带动效果开展查询分析。",
    metrics: ["consume_heat", "consume_orders", "consume_ticket", "activity_spread_index"],
  },
  {
    id: "emergency",
    label: "应急指挥",
    description: "围绕视频监控、风险预警、事件处置和会商调用开展运行查询。",
    metrics: ["video_online_rate", "warning_events", "event_closed_rate", "video_meeting_calls"],
  },
  {
    id: "foundation",
    label: "底座与智能应用",
    description: "围绕数据接入、共享开通、指标复用和智能应用运行开展查询分析。",
    metrics: ["data_access_total", "shared_service_total", "indicator_reuse_count", "ai_query_calls"],
  },
  {
    id: "area-query",
    label: "区域数据查询",
    description: "面向省、市州、区县开展区域客流量和区域游客画像查询、统计和导出。",
    metrics: ["area_flow", "area_visitor_profile"],
  },
  {
    id: "scenic-query",
    label: "景区数据查询",
    description: "面向景区开展游客接待、游客画像和游客预约数据查询、统计和导出。",
    metrics: ["scenic_reception", "scenic_visitor_profile", "scenic_reservation"],
  },
];

const queryResultViews = [
  { id: "trend", label: "趋势分析", icon: BarChart3 },
  { id: "distribution", label: "分布分析", icon: MapPinned },
  { id: "pivot", label: "多维透视", icon: SlidersHorizontal },
  { id: "detail", label: "明细结果", icon: Table2 },
];

const queryTimePresetOptions = [
  { id: "year-2026", label: "2026年" },
  { id: "month-2026-07", label: "2026年7月" },
  { id: "recent-7", label: "近7日" },
  { id: "holiday-2026", label: "节假日" },
];

const querySourceFactorMap = {
  综合口径: 1,
  百度: 1.03,
  电信: 0.94,
  高德: 0.89,
  闸机: 1,
  票务: 0.97,
  PMS: 1,
  监控平台: 1,
  政府智能体: 1,
};

const queryResourceTypeFactorMap = {
  全部: 1,
  景区: 1,
  街区: 0.42,
  度假区: 0.36,
  乡村旅游点: 0.48,
  酒店民宿: 0.33,
  文化场馆: 0.24,
  旅行社: 0.18,
  导游: 0.15,
  餐饮主体: 0.27,
  购物主体: 0.21,
  旅游服务主体: 0.23,
};

const queryMetricCatalog = [
  {
    id: "resource_total",
    label: "资源主体总量",
    theme: "resource",
    unit: "个",
    mode: "resource",
    description: "反映全省纳入统一资源目录的资源主体总量。",
    definition: "按资源主数据统一编码去重统计，覆盖旅游资源、住宿资源、文化资源和涉旅主体。",
    dimensions: ["市州", "资源类型", "等级", "收费情况"],
    sources: ["资源主数据"],
    granularityOptions: ["当前"],
    relatedMetricName: "发布客流",
  },
  {
    id: "scenic_count",
    label: "景区数量",
    theme: "resource",
    unit: "个",
    mode: "resource",
    resourceCategories: ["景区"],
    description: "反映景区资源纳管数量及结构分布。",
    definition: "按景区主数据标准名称和资源编码去重统计。",
    dimensions: ["市州", "等级", "收费情况", "预约情况"],
    sources: ["旅游资源主数据"],
    granularityOptions: ["当前"],
    relatedMetricName: "日累计入园人数",
  },
  {
    id: "hotel_count",
    label: "酒店民宿数量",
    theme: "resource",
    unit: "个",
    mode: "resource",
    resourceCategories: ["酒店民宿"],
    description: "反映全省酒店民宿资源纳管情况。",
    definition: "按住宿资源主数据去重统计，覆盖酒店、民宿、客栈等主体。",
    dimensions: ["市州", "等级", "经营状态", "OTA上线情况"],
    sources: ["住宿资源主数据"],
    granularityOptions: ["当前"],
    relatedMetricName: "住宿入住率",
  },
  {
    id: "cultural_venue_count",
    label: "文化场馆数量",
    theme: "resource",
    unit: "个",
    mode: "resource",
    resourceCategories: ["文化场馆"],
    description: "反映博物馆、文化馆、非遗展示馆等文化资源纳管数量。",
    definition: "按文化资源主数据标准编码去重统计。",
    dimensions: ["市州", "等级", "场馆类型"],
    sources: ["文化资源主数据"],
    granularityOptions: ["当前"],
  },
  {
    id: "travel_subject_count",
    label: "涉旅主体数量",
    theme: "resource",
    unit: "个",
    mode: "resource",
    resourceCategories: ["旅行社", "导游", "餐饮主体", "购物主体", "旅游服务主体"],
    description: "反映旅行社、导游及涉旅经营主体纳管数量。",
    definition: "按主体统一编码去重统计。",
    dimensions: ["市州", "主体类型", "经营状态"],
    sources: ["涉旅主体主数据"],
    granularityOptions: ["当前"],
  },
  {
    id: "lbs_flow",
    label: "LBS客流",
    theme: "flow",
    unit: "万人次",
    mode: "timeseries",
    description: "反映基于 LBS 数据识别的游客流动规模。",
    definition: "按百度、电信等位置服务数据统一口径加工形成省、市州和重点景区客流指标。",
    dimensions: ["市州", "月份", "来源渠道", "资源类型"],
    sources: ["综合口径", "百度", "电信"],
    granularityOptions: ["日", "月"],
    monthlyBase: [920, 980, 1080, 1210, 1360, 1440, 1580],
    relatedMetricName: "发布客流",
  },
  {
    id: "gate_flow",
    label: "闸机客流",
    theme: "flow",
    unit: "万人次",
    mode: "timeseries",
    description: "反映重点景区闸机或票务核销形成的入园客流规模。",
    definition: "优先采用闸机入园人数和票务核销人数，按 5 分钟滚动汇总到日/月粒度。",
    dimensions: ["市州", "月份", "景区等级", "资源类型"],
    sources: ["闸机", "票务"],
    granularityOptions: ["日", "月"],
    monthlyBase: [310, 328, 352, 384, 426, 452, 486],
    relatedMetricName: "日累计入园人数",
  },
  {
    id: "park_visitors",
    label: "实时在园人数",
    theme: "flow",
    unit: "万人",
    mode: "timeseries",
    description: "反映重点景区在园游客承载水平。",
    definition: "按实时入园人数减去实时出园人数形成在园人数，并按时间窗口汇总。",
    dimensions: ["市州", "月份", "景区等级"],
    sources: ["闸机"],
    granularityOptions: ["日", "月"],
    monthlyBase: [18.2, 18.9, 19.6, 20.4, 21.3, 22.1, 23.6],
    relatedMetricName: "实时在园人数",
  },
  {
    id: "visitor_origin_ratio",
    label: "省外游客占比",
    theme: "flow",
    unit: "%",
    mode: "timeseries",
    description: "反映省外游客在全部游客中的占比变化。",
    definition: "按 LBS、订单和住宿游客来源地融合识别，形成省外游客占比指标。",
    dimensions: ["市州", "月份", "来源渠道"],
    sources: ["综合口径", "百度", "电信"],
    granularityOptions: ["日", "月"],
    monthlyBase: [42.1, 43.5, 45.2, 46.8, 48.4, 49.1, 50.2],
    relatedMetricName: "游客来源占比",
  },
  {
    id: "ticket_orders",
    label: "门票订单量",
    theme: "operation",
    unit: "万单",
    mode: "timeseries",
    description: "反映景区门票、观光车票等订单规模。",
    definition: "按票务订单去重汇总，支持按渠道、区域、景区等级查询。",
    dimensions: ["市州", "月份", "渠道", "景区等级"],
    sources: ["票务", "综合口径"],
    granularityOptions: ["日", "月"],
    monthlyBase: [118, 126, 138, 149, 166, 174, 188],
  },
  {
    id: "ticket_revenue",
    label: "门票收入",
    theme: "operation",
    unit: "亿元",
    mode: "timeseries",
    description: "反映景区门票及配套票种形成的收入规模。",
    definition: "按有效订单金额汇总，剔除退款和无效订单。",
    dimensions: ["市州", "月份", "渠道", "景区等级"],
    sources: ["票务", "综合口径"],
    granularityOptions: ["日", "月"],
    monthlyBase: [1.28, 1.35, 1.48, 1.63, 1.82, 1.96, 2.14],
  },
  {
    id: "parking_capacity",
    label: "停车场总车位",
    theme: "operation",
    unit: "个",
    mode: "timeseries",
    description: "反映重点景区停车供给总量。",
    definition: "按已纳管停车场基础台账汇总车位总数，日常按景区和区域展示。",
    dimensions: ["市州", "景区等级", "资源类型"],
    sources: ["停车场台账"],
    granularityOptions: ["月"],
    monthlyBase: [21480, 21520, 21580, 21630, 21720, 21810, 21940],
  },
  {
    id: "parking_vacancy_rate",
    label: "停车空余率",
    theme: "operation",
    unit: "%",
    mode: "timeseries",
    description: "反映重点景区停车资源空余情况。",
    definition: "按实时空车位除以停车场总车位计算。",
    dimensions: ["市州", "月份", "景区等级"],
    sources: ["停车场实时数据"],
    granularityOptions: ["日", "月"],
    monthlyBase: [36.2, 35.8, 34.7, 32.4, 30.6, 29.4, 27.8],
  },
  {
    id: "stay_visitors",
    label: "过夜游客量",
    theme: "stay",
    unit: "万人",
    mode: "timeseries",
    description: "反映全省及各市州过夜游客规模。",
    definition: "按住宿旅客入住记录和住宿订单去重汇总形成。",
    dimensions: ["市州", "月份", "住宿类型", "来源渠道"],
    sources: ["PMS", "综合口径"],
    granularityOptions: ["日", "月"],
    monthlyBase: [74, 78, 85, 92, 104, 112, 126],
    relatedMetricName: "过夜游客量",
  },
  {
    id: "occupancy_rate",
    label: "住宿入住率",
    theme: "stay",
    unit: "%",
    mode: "timeseries",
    description: "反映住宿经营主体有效房量使用水平。",
    definition: "按有效房量与已入住间夜测算，剔除停业和不可售房源。",
    dimensions: ["市州", "月份", "住宿类型"],
    sources: ["PMS", "综合口径"],
    granularityOptions: ["日", "月"],
    monthlyBase: [58.4, 60.1, 63.7, 68.3, 72.8, 74.6, 77.2],
    relatedMetricName: "住宿入住率",
  },
  {
    id: "avg_room_rate",
    label: "平均房价",
    theme: "stay",
    unit: "元",
    mode: "timeseries",
    description: "反映住宿经营市场平均成交价格水平。",
    definition: "按已售间夜金额除以间夜数计算。",
    dimensions: ["市州", "月份", "住宿类型", "来源渠道"],
    sources: ["PMS", "OTA"],
    granularityOptions: ["日", "月"],
    monthlyBase: [286, 294, 306, 328, 351, 364, 378],
    relatedMetricName: "平均房价",
  },
  {
    id: "pms_connected_hotels",
    label: "PMS接入酒店数",
    theme: "stay",
    unit: "个",
    mode: "timeseries",
    description: "反映接入住宿经营数据的酒店民宿主体数量。",
    definition: "按完成接口联通、已产生经营数据的住宿主体去重统计。",
    dimensions: ["市州", "月份", "住宿类型"],
    sources: ["PMS"],
    granularityOptions: ["月"],
    monthlyBase: [62, 71, 84, 98, 113, 127, 138],
  },
  {
    id: "consume_heat",
    label: "消费热度指数",
    theme: "consume",
    unit: "分",
    mode: "timeseries",
    description: "综合反映文旅消费活跃度和带动水平。",
    definition: "按消费笔数、客单价、交易活跃度和节假日权重形成综合指数。",
    dimensions: ["市州", "月份", "资源类型", "来源渠道"],
    sources: ["综合口径", "高德"],
    granularityOptions: ["日", "月"],
    monthlyBase: [72.4, 73.6, 75.2, 78.8, 82.4, 85.1, 87.6],
    relatedMetricName: "消费热度指数",
  },
  {
    id: "consume_orders",
    label: "消费笔数",
    theme: "consume",
    unit: "万笔",
    mode: "timeseries",
    description: "反映文旅消费交易笔数规模。",
    definition: "按纳管消费数据源有效消费笔数汇总。",
    dimensions: ["市州", "月份", "资源类型", "来源渠道"],
    sources: ["综合口径", "高德"],
    granularityOptions: ["日", "月"],
    monthlyBase: [62, 66, 71, 79, 86, 91, 98],
  },
  {
    id: "consume_ticket",
    label: "平均客单价",
    theme: "consume",
    unit: "元",
    mode: "timeseries",
    description: "反映文旅消费平均客单价水平。",
    definition: "按交易总额除以消费笔数形成平均客单价。",
    dimensions: ["市州", "月份", "资源类型"],
    sources: ["综合口径", "高德"],
    granularityOptions: ["日", "月"],
    monthlyBase: [128, 131, 134, 138, 141, 143, 146],
  },
  {
    id: "activity_spread_index",
    label: "活动传播指数",
    theme: "consume",
    unit: "分",
    mode: "timeseries",
    description: "反映重点活动在互联网端的传播热度和覆盖效果。",
    definition: "综合阅读量、互动量、平台覆盖和关联客流带动形成传播指数。",
    dimensions: ["市州", "月份", "活动类型"],
    sources: ["互联网公开数据", "活动管理系统"],
    granularityOptions: ["日", "月"],
    monthlyBase: [61, 63, 68, 72, 78, 84, 88],
    relatedMetricName: "活动传播指数",
  },
  {
    id: "video_online_rate",
    label: "视频在线率",
    theme: "emergency",
    unit: "%",
    mode: "timeseries",
    description: "反映纳管景区视频监控在线接入和可用情况。",
    definition: "按在线摄像头数量除以纳管摄像头总数计算。",
    dimensions: ["市州", "月份", "责任单位"],
    sources: ["监控平台"],
    granularityOptions: ["日", "月"],
    monthlyBase: [94.1, 94.6, 95.2, 95.8, 96.2, 96.5, 96.9],
    relatedMetricName: "视频在线率",
  },
  {
    id: "warning_events",
    label: "风险预警数",
    theme: "emergency",
    unit: "条",
    mode: "timeseries",
    description: "反映客流、停车、气象和事件上报等风险预警数量。",
    definition: "按风险规则触发的有效预警事件汇总。",
    dimensions: ["市州", "月份", "风险等级", "事件类型"],
    sources: ["监控平台", "综合口径"],
    granularityOptions: ["日", "月"],
    monthlyBase: [26, 24, 22, 27, 31, 34, 29],
    relatedMetricName: "活动风险等级",
  },
  {
    id: "event_closed_rate",
    label: "事件处置闭环率",
    theme: "emergency",
    unit: "%",
    mode: "timeseries",
    description: "反映应急事件闭环处置和督办情况。",
    definition: "按已办结事件数除以事件总数计算，逾期事件单独监测。",
    dimensions: ["市州", "月份", "责任单位"],
    sources: ["监控平台"],
    granularityOptions: ["日", "月"],
    monthlyBase: [87.6, 88.1, 89.2, 90.4, 91.3, 92.1, 92.8],
    relatedMetricName: "事件处置闭环率",
  },
  {
    id: "video_meeting_calls",
    label: "视频会商调用次数",
    theme: "emergency",
    unit: "次",
    mode: "timeseries",
    description: "反映景区视频会商和应急联动使用频次。",
    definition: "按会商系统记录的有效调用次数统计。",
    dimensions: ["市州", "月份", "责任单位"],
    sources: ["监控平台"],
    granularityOptions: ["日", "月"],
    monthlyBase: [36, 42, 48, 53, 61, 66, 72],
  },
  {
    id: "data_access_total",
    label: "接入数据源数量",
    theme: "foundation",
    unit: "项",
    mode: "timeseries",
    description: "反映已纳入底座统一接入管理的数据源数量。",
    definition: "按数据接入清单中的有效接入项统计。",
    dimensions: ["月份", "数据类型", "来源渠道"],
    sources: ["数据接入清单"],
    granularityOptions: ["月"],
    monthlyBase: [28, 31, 36, 39, 45, 48, 54],
  },
  {
    id: "shared_service_total",
    label: "共享开通事项数",
    theme: "foundation",
    unit: "项",
    mode: "timeseries",
    description: "反映已审核开通的数据共享事项数量。",
    definition: "按共享申请清单中状态为已开通的事项累计统计。",
    dimensions: ["月份", "共享类型", "责任单位"],
    sources: ["共享申请清单"],
    granularityOptions: ["月"],
    monthlyBase: [16, 19, 22, 26, 29, 33, 38],
  },
  {
    id: "indicator_reuse_count",
    label: "指标复用次数",
    theme: "foundation",
    unit: "次",
    mode: "timeseries",
    description: "反映统一指标在驾驶舱、问数、报告和共享服务中的复用调用情况。",
    definition: "按统一指标服务调用日志累计统计。",
    dimensions: ["月份", "应用场景", "责任单位"],
    sources: ["统一指标服务"],
    granularityOptions: ["月"],
    monthlyBase: [260, 318, 386, 452, 538, 624, 708],
  },
  {
    id: "ai_query_calls",
    label: "智能问数调用量",
    theme: "foundation",
    unit: "次",
    mode: "timeseries",
    description: "反映政府智能体、景区智能体和酒店智能体的智能问数调用情况。",
    definition: "按统一问数入口、政府智能体和业务智能体调用日志累计统计。",
    dimensions: ["月份", "应用场景", "责任单位"],
    sources: ["政府智能体", "黄小西智能体", "酒店智能体"],
    granularityOptions: ["日", "月"],
    monthlyBase: [1280, 1460, 1690, 1880, 2120, 2280, 2416],
  },
  {
    id: "area_flow",
    label: "区域客流量",
    theme: "area-query",
    unit: "万人次",
    mode: "timeseries",
    description: "支持省、市州、区县区域客流量对接数据查询、统计和导出。",
    definition: "汇聚 LBS、运营商、交通和重点场所客流数据，按区域层级统一形成区域客流统计指标。",
    dimensions: ["区域层级", "市州", "区县", "月份", "来源渠道"],
    sources: ["综合口径", "百度", "电信", "高德"],
    granularityOptions: ["日", "周", "月", "季度", "半年", "年", "节假日"],
    monthlyBase: [860, 936, 1048, 1186, 1308, 1422, 1536],
    relatedMetricName: "发布客流",
  },
  {
    id: "area_visitor_profile",
    label: "区域游客画像",
    theme: "area-query",
    unit: "万人次",
    mode: "timeseries",
    description: "支持省、市州、区县区域游客画像对接数据查询、统计和导出。",
    definition: "围绕游客性别、年龄、出行方式、停留时长、来源地、旅游半径、夜游情况和平均旅游城市等画像维度统计。",
    dimensions: ["性别", "年龄", "出行方式", "游客停留时长", "游客来源地", "旅游半径", "夜游情况", "平均旅游城市"],
    sources: ["综合口径", "百度", "电信", "高德"],
    granularityOptions: ["日", "周", "月", "季度", "半年", "年", "节假日"],
    monthlyBase: [430, 468, 524, 593, 654, 711, 768],
    relatedMetricName: "游客来源占比",
  },
  {
    id: "scenic_reception",
    label: "游客接待",
    theme: "scenic-query",
    unit: "万人次",
    mode: "timeseries",
    description: "支持景区接待客流数据查询、统计和导出，覆盖总客流、实时在园和日累计接待等。",
    definition: "对接景区闸机、票务、预约和 LBS 数据，按小时、日、周、月、季度、半年、年及节假日维度统计。",
    dimensions: ["景区", "接待类型", "景区等级", "月份"],
    sources: ["综合口径", "闸机", "票务", "百度"],
    granularityOptions: ["小时", "日", "周", "月", "季度", "半年", "年", "节假日"],
    monthlyBase: [268, 286, 314, 352, 396, 426, 468],
    relatedMetricName: "日累计入园人数",
  },
  {
    id: "scenic_visitor_profile",
    label: "游客画像",
    theme: "scenic-query",
    unit: "万人次",
    mode: "timeseries",
    description: "支持景区游客画像对接数据查询、统计和导出。",
    definition: "围绕游客性别、年龄、停留时长和来源地进行景区画像统计，支持日、月、年、节假日维度。",
    dimensions: ["景区", "性别", "年龄", "游客停留时长", "游客来源地"],
    sources: ["综合口径", "百度", "电信"],
    granularityOptions: ["日", "月", "年", "节假日"],
    monthlyBase: [132, 144, 158, 176, 198, 213, 234],
    relatedMetricName: "游客来源占比",
  },
  {
    id: "scenic_reservation",
    label: "游客预约",
    theme: "scenic-query",
    unit: "万单",
    mode: "timeseries",
    description: "支持景区游客预约对接数据查询、统计和导出。",
    definition: "汇聚景区预约平台、票务平台和分时预约记录，支持日、月、年维度统计。",
    dimensions: ["景区", "预约渠道", "景区等级", "月份"],
    sources: ["票务", "预约平台", "综合口径"],
    granularityOptions: ["日", "月", "年"],
    monthlyBase: [86, 92, 104, 118, 132, 146, 162],
    relatedMetricName: "门票订单量",
  },
];

const queryReportTemplates = [
  {
    id: "report-1",
    title: "全省景区资源结构分析",
    theme: "resource",
    metricId: "scenic_count",
    granularity: "当前",
    timePreset: "year-2026",
    region: "全省",
    dimension: "等级",
    source: "旅游资源主数据",
    summary: "查看全省景区数量，按等级、收费情况和市州分布进行分析。",
  },
  {
    id: "report-2",
    title: "全省酒店民宿资源分布",
    theme: "resource",
    metricId: "hotel_count",
    granularity: "当前",
    timePreset: "year-2026",
    region: "全省",
    dimension: "市州",
    source: "住宿资源主数据",
    summary: "查看酒店民宿资源数量、等级结构和区域分布。",
  },
  {
    id: "report-3",
    title: "7月每日 LBS 客流",
    theme: "flow",
    metricId: "lbs_flow",
    granularity: "日",
    timePreset: "month-2026-07",
    region: "全省",
    dimension: "市州",
    source: "综合口径",
    summary: "查看 2026 年 7 月每日 LBS 客流趋势和市州分布。",
  },
  {
    id: "report-4",
    title: "2026年累计客流分析",
    theme: "flow",
    metricId: "lbs_flow",
    granularity: "月",
    timePreset: "year-2026",
    region: "全省",
    dimension: "月份",
    source: "综合口径",
    summary: "查看 2026 年累计客流变化趋势、峰值月份和地州对比。",
  },
  {
    id: "report-area-query",
    title: "区域数据查询报表",
    theme: "area-query",
    metricId: "area_flow",
    granularity: "日",
    timePreset: "month-2026-07",
    region: "全省",
    dimension: "市州",
    source: "综合口径",
    queryModule: "query-area",
    summary: "点击后直接进入区域数据查询详情页，查看区域客流和游客画像。",
  },
  {
    id: "report-scenic-query",
    title: "景区数据查询报表",
    theme: "scenic-query",
    metricId: "scenic_reception",
    granularity: "日",
    timePreset: "month-2026-07",
    region: "黄果树旅游区",
    dimension: "景区",
    source: "综合口径",
    queryModule: "query-scenic",
    summary: "点击后直接进入景区数据查询详情页，查看游客接待、画像和预约。",
  },
  {
    id: "report-5",
    title: "门票收入渠道对比",
    theme: "operation",
    metricId: "ticket_revenue",
    granularity: "月",
    timePreset: "year-2026",
    region: "全省",
    dimension: "渠道",
    source: "综合口径",
    summary: "查看门票收入按月份、渠道和景区等级的分布情况。",
  },
  {
    id: "report-6",
    title: "视频在线率监测",
    theme: "emergency",
    metricId: "video_online_rate",
    granularity: "日",
    timePreset: "recent-7",
    region: "全省",
    dimension: "市州",
    source: "监控平台",
    summary: "查看近 7 日视频在线率变化、分市州分布和责任单位情况。",
  },
];

const queryDimensionFallback = {
  resource: "市州",
  flow: "市州",
  operation: "渠道",
  stay: "市州",
  consume: "市州",
  emergency: "市州",
  foundation: "月份",
  "area-query": "市州",
  "scenic-query": "景区",
};

const assetDetailBlueprints = {
  "旅游资源主数据": {
    summary: "面向景区、街区、度假区、乡村旅游点、特色打卡点等旅游资源对象，统一沉淀标准名称、编码、分类、区位、服务属性和关联关系，形成全省旅游资源统一底账。",
    includes: ["景区", "旅游休闲街区", "旅游度假区", "乡村旅游点", "特色打卡点", "游客服务配套资源"],
    fields: ["资源唯一编码", "标准名称", "别名/简称", "资源分类", "质量等级", "营业状态", "省市区县", "详细地址", "经纬度", "开放时间", "门票价格", "联系电话", "资源介绍"],
    outputs: ["资源目录查询", "资源底图", "智能推荐调用", "导览问答底数"],
  },
  "住宿资源主数据": {
    summary: "面向酒店、民宿、客栈等住宿主体，统一沉淀注册信息、经营信息、房态基础信息和平台接入信息，形成住宿资源统一底账。",
    includes: ["酒店", "民宿", "客栈", "度假酒店", "精品民宿"],
    fields: ["统一社会信用代码", "经营单位名称", "场所类别", "实际经营地址", "客房数", "床位数", "从业人员数", "等级评定", "OTA 上线情况", "公安旅馆治安系统接入状态"],
    outputs: ["住宿目录", "住宿主体画像", "酒店智能体底数", "经营监测基础层"],
  },
  "涉旅主体主数据": {
    summary: "统一沉淀旅行社、导游、涉旅经营主体等对象标准身份信息，支撑监管、信用和共享交换。",
    includes: ["旅行社", "导游", "餐饮主体", "购物主体", "旅游服务主体"],
    fields: ["主体编码", "主体名称", "主体类型", "信用等级", "所属地区", "经营状态", "许可证信息", "联系方式"],
    outputs: ["涉旅主体目录", "信用监管画像", "市场监管底账"],
  },
  "文化资源主数据": {
    summary: "统一沉淀博物馆、文化馆、非遗展示馆、美术馆等文化资源底账，支撑文化资源展示和知识库建设。",
    includes: ["博物馆", "文化馆", "非遗展示馆", "美术馆", "数字文化体验中心"],
    fields: ["资源编码", "资源名称", "资源类型", "隶属层级", "题材类型", "开放时间", "展陈面积", "位置坐标", "预约规则"],
    outputs: ["文化资源目录", "数字文化展示", "文化知识库底数"],
  },
  "景区客流主题数据集": {
    summary: "围绕景区实时入园、出园、在园、累计入园和分时客流等数据形成统一主题集，是节假日监测和应急指挥的重要数据资产。",
    includes: ["实时入园人数", "实时出园人数", "在园人数", "日累计入园人数", "分时段客流", "闸机点位信息"],
    fields: ["景区编码", "统计时间", "入园人数", "出园人数", "在园人数", "累计入园人数", "闸机点位", "来源系统"],
    outputs: ["发布客流", "景区客流趋势", "承载预警分析", "节假日专题数据集"],
  },
  "住宿经营运行主题集": {
    summary: "围绕入住、离店、房态、房价、间夜和渠道数据形成统一主题集，用于过夜游客分析和住宿经营监测。",
    includes: ["入住数据", "离店数据", "间夜数据", "房态数据", "房价数据", "渠道数据"],
    fields: ["住宿主体编码", "入住日期", "已售房间数", "有效房量", "平均房价", "入住人数", "渠道来源"],
    outputs: ["入住率", "平均房价", "过夜游客量", "住宿运行专题"],
  },
  "文化市场监管主题集": {
    summary: "整合文化方向市场监督执法数据，支撑文化主体监管、执法治理和异常预警。",
    includes: ["文娱场所管理", "营业/停业/变更审核", "视频监控", "法律法规", "执法信息"],
    fields: ["主体名称", "场所类别", "审批状态", "执法事件", "视频状态", "更新时间"],
    outputs: ["文化市场监管专题", "执法治理分析"],
  },
  "旅游市场监管主题集": {
    summary: "整合旅游方向监管数据，覆盖民宿、导游、旅行社、景区、购物店和餐饮服务等对象。",
    includes: ["民宿监管", "导游监管", "旅行社监管", "购物店监管", "合同列表", "舆情管理", "保单管理"],
    fields: ["主体编码", "主体类型", "监管事件", "合同状态", "舆情状态", "更新时间"],
    outputs: ["旅游市场监管专题", "舆情与执法联动分析"],
  },
  "文旅信用主题集": {
    summary: "围绕旅行社、导游等涉旅主体形成统一信用监管资产，支撑信用画像和联合监管。",
    includes: ["信用评分", "信用等级", "失信记录", "表彰记录", "投诉处置结果"],
    fields: ["主体编码", "信用等级", "信用分值", "风险标签", "评价时间"],
    outputs: ["信用画像", "风险预警", "联合监管支持"],
  },
  "安全预警与事件处置主题集": {
    summary: "整合天气、交通、人流、视频监控、停车和事件上报信息，形成安全预警与应急处置统一资产。",
    includes: ["天气风险", "交通风险", "客流风险", "停车风险", "事件告警", "处置进展"],
    fields: ["事件编号", "风险等级", "事件类型", "发生时间", "处置状态", "调度指令"],
    outputs: ["主动预警", "事件调度", "会商研判专题"],
  },
  "活动传播评估主题集": {
    summary: "围绕重点活动的传播力、带动客流和消费影响形成活动传播评估资产。",
    includes: ["活动基础信息", "平台传播数据", "互动量", "客流联动数据", "消费带动数据"],
    fields: ["活动名称", "活动时间", "传播指数", "话题声量", "活动客流", "消费热度"],
    outputs: ["活动传播指数", "传播复盘报告", "活动影响评估"],
  },
  "全域运行监测主题集": {
    summary: "综合全省游客数量、收入情况、景区运营状况和区域运行态势形成全域运行监测资产。",
    includes: ["游客数量", "收入情况", "景区运行", "区域运行态势", "节假日运行情况"],
    fields: ["区域编码", "统计日期", "游客总量", "旅游收入", "重点景区运行状态"],
    outputs: ["全域监测专题", "领导驾驶舱支撑", "综合运行研判"],
  },
  "LBS客流主题集": {
    summary: "基于百度客流、电信客流等位置服务数据形成跨区域客流监测资产，用于客源地和区域流动分析。",
    includes: ["省级客流", "市州客流", "区县客流", "重点景区客流", "迁徙趋势"],
    fields: ["区域层级", "区域名称", "统计日期", "客流规模", "客源地占比"],
    outputs: ["客源地分析", "区域客流对比", "趋势研判"],
  },
  "文旅消费热度主题集": {
    summary: "围绕高德消费、OTA 订单和贵客荟交易数据形成消费热度资产，支撑消费分析和活动带动评估。",
    includes: ["消费笔数", "客单价", "交易活跃度", "节假日消费波动", "重点景区消费热度"],
    fields: ["区域名称", "统计日期", "消费热度指数", "消费笔数", "客单价"],
    outputs: ["消费热度指数", "消费运行分析", "营销评估"],
  },
  "游客画像标签库": {
    summary: "围绕游客来源地、年龄、性别、团队/散客等维度形成统一游客画像标签资产。",
    includes: ["客源地标签", "年龄分段标签", "性别标签", "游客类型标签", "消费偏好标签"],
    fields: ["游客ID", "客源地", "年龄段", "性别", "游客类型", "偏好标签"],
    outputs: ["精准营销", "游客画像分析", "智能推荐支持"],
  },
  "涉旅主体信用标签库": {
    summary: "围绕涉旅主体的信用情况、监管状态和风险级别形成统一标签资产。",
    includes: ["信用等级标签", "风险标签", "投诉标签", "处罚标签", "重点关注标签"],
    fields: ["主体编码", "信用等级", "风险标签", "投诉次数", "处罚状态"],
    outputs: ["分层监管", "信用预警", "联合惩戒支持"],
  },
  "酒店知识库": {
    summary: "围绕酒店主体基础信息、入住政策、经营规则和常见问答形成酒店知识资产。",
    includes: ["酒店基础信息", "入住政策", "经营知识", "服务流程", "常见问题"],
    fields: ["知识主题", "知识内容", "适用对象", "更新时间"],
    outputs: ["酒店问答", "经营助手", "服务检索"],
  },
  "景区知识库": {
    summary: "围绕景区基础信息、游玩攻略、活动安排和常见问题形成景区知识资产。",
    includes: ["景区介绍", "游玩路线", "开放信息", "活动信息", "问答知识"],
    fields: ["景区名称", "知识主题", "知识内容", "适用场景", "更新时间"],
    outputs: ["行程规划", "伴游问答", "智能推荐"],
  },
  "数字文化资源知识库": {
    summary: "围绕文化资源、非遗展示和数字化内容形成统一知识资产，支撑文化问答和知识检索。",
    includes: ["文化资源介绍", "非遗知识", "展陈内容", "数字展示素材"],
    fields: ["资源名称", "题材类型", "知识内容", "展示方式"],
    outputs: ["文化问答", "知识检索", "数字文化展示"],
  },
  "视频监控专题资源": {
    summary: "围绕景区视频监控点位、画面流和会商调用形成视频专题资产，是应急指挥的重要组成部分。",
    includes: ["重点点位视频流", "监控点位台账", "视频在线状态", "会商调用信息"],
    fields: ["点位编码", "景区名称", "点位名称", "视频状态", "更新时间"],
    outputs: ["视频会商", "事件核验", "应急监测"],
  },
  "气象风险专题集": {
    summary: "围绕气象预报和气象预警数据形成风险专题资产，用于主动预警和出行提示。",
    includes: ["气象预报", "气象预警", "风险等级", "影响区域"],
    fields: ["预警时间", "风险类型", "风险等级", "影响区域"],
    outputs: ["气象风险识别", "主动预警", "出行提示"],
  },
  "统一指标口径库": {
    summary: "统一沉淀指标名称、定义、口径、来源、责任单位和适用场景，形成全厅统一指标资产。",
    includes: ["指标名称", "指标说明", "口径说明", "数据来源", "责任单位", "更新频率", "应用场景"],
    fields: ["指标编码", "指标名称", "主题域", "口径说明", "数据来源", "责任单位"],
    outputs: ["指标资产", "智能问数底座", "驾驶舱指标复用"],
  },
  "旅游资源主数据查询服务": {
    summary: "将旅游资源主数据以目录查询和 API 方式提供出来，形成可调用的服务资产。",
    includes: ["资源目录查询", "资源详情查询", "资源编码检索", "资源关系映射"],
    fields: ["服务名称", "调用方式", "资源范围", "授权方式"],
    outputs: ["API 服务", "目录服务", "共享交换服务"],
  },
  "景区客流专题服务": {
    summary: "将景区客流主题资产和统一指标封装为可复用服务，支撑驾驶舱、节假日调度和专题分析。",
    includes: ["客流指标查询", "客流趋势服务", "承载预警服务"],
    fields: ["景区编码", "时间维度", "客流指标", "发布状态"],
    outputs: ["客流接口", "专题数据服务", "预警服务"],
  },
  "应急视频会商服务": {
    summary: "围绕视频监控专题资源形成视频会商和调用服务，支撑事件处置和会商研判。",
    includes: ["视频流调用", "会商接入", "事件核验", "调度联动"],
    fields: ["点位名称", "视频地址", "调用权限", "在线状态"],
    outputs: ["视频服务", "会商服务", "应急调度支撑"],
  },
  "智能问数与知识检索服务": {
    summary: "围绕统一指标和知识资产提供自然语言问数、问策和知识检索能力。",
    includes: ["智能问数", "知识问答", "专题检索", "报告辅助生成"],
    fields: ["服务名称", "知识范围", "指标范围", "授权方式"],
    outputs: ["智能问数", "知识问答", "报告支持"],
  },
};

const assetTypeFallbackBlueprints = {
  "主数据资产": {
    summary: "围绕统一对象形成标准底账，解决多系统对象不一致的问题。",
    includes: ["统一编码", "标准名称", "基础属性", "区位信息", "治理信息"],
    fields: ["对象编码", "标准名称", "分类", "状态", "更新时间"],
    outputs: ["目录查询", "主数据共享", "对象统一关联"],
  },
  "主题数据资产": {
    summary: "围绕业务场景形成的专题化数据沉淀，可直接用于监测、分析和调度。",
    includes: ["业务指标", "专题明细", "时间序列", "区域维度"],
    fields: ["主题对象", "统计时间", "核心指标", "来源系统"],
    outputs: ["专题数据集", "分析支撑", "专题共享"],
  },
  "指标资产": {
    summary: "围绕指标定义和口径形成统一资产，确保不同系统和场景口径一致。",
    includes: ["指标定义", "口径说明", "来源说明", "责任单位"],
    fields: ["指标名称", "口径说明", "来源", "责任单位"],
    outputs: ["指标查询", "智能问数", "驾驶舱复用"],
  },
  "标签资产": {
    summary: "围绕对象画像和分层识别形成统一标签资产。",
    includes: ["画像标签", "风险标签", "偏好标签"],
    fields: ["对象标识", "标签名称", "标签值", "更新时间"],
    outputs: ["画像分析", "标签服务", "精准运营支持"],
  },
  "知识资产": {
    summary: "围绕业务知识和内容沉淀形成知识库资产。",
    includes: ["知识主题", "问答内容", "规则说明", "场景知识"],
    fields: ["知识标题", "知识内容", "适用场景", "更新时间"],
    outputs: ["问答服务", "知识检索", "智能应用支撑"],
  },
  "服务资产": {
    summary: "将数据资产以 API、查询、专题服务等方式封装为可使用能力。",
    includes: ["API 服务", "目录服务", "指标服务", "数据共享服务"],
    fields: ["服务名称", "调用方式", "授权方式", "服务对象"],
    outputs: ["系统调用", "共享交换", "应用集成"],
  },
};

const assetGovernanceProfiles = {
  主数据资产: { department: "资源开发处", openLevel: "厅内共享", shareStatus: "已发布", serviceMode: "目录查询 + API" },
  主题数据资产: { department: "数据办", openLevel: "按专题共享", shareStatus: "部分开放", serviceMode: "专题数据集 + 指标服务" },
  指标资产: { department: "数据办", openLevel: "统一发布", shareStatus: "已发布", serviceMode: "指标服务 + 智能问数" },
  标签资产: { department: "宣传推广处", openLevel: "脱敏共享", shareStatus: "按申请开放", serviceMode: "标签服务" },
  知识资产: { department: "信息中心", openLevel: "按应用授权", shareStatus: "已接入应用", serviceMode: "知识检索 + 智能问答" },
  服务资产: { department: "信息中心", openLevel: "按接口授权", shareStatus: "已开通服务", serviceMode: "API + 数据服务" },
};

businessSystems.forEach((item, index) => {
  item.id = `sys-${index + 1}`;
  item.ownerDept =
    item.ownerDept ??
    (item.type === "业务系统"
      ? "对应业务处室"
      : item.type === "智能应用"
        ? "信息中心"
        : item.type === "在建系统"
          ? "市场管理处"
          : "数据办");
  item.accessMode = item.accessMode ?? (item.type === "外部数据" ? "数据交换接入" : item.type === "智能应用" ? "应用接入" : "系统直连");
});

dataAccessCatalog.forEach((item, index) => {
  item.id = `access-${index + 1}`;
  item.accessMode =
    item.accessMode ??
    (item.name.includes("基础数据") || item.name.includes("知识库")
      ? "线下导入"
      : item.name.includes("PMS") || item.name.includes("订单") || item.name.includes("客流") || item.name.includes("消费")
        ? "数据库同步"
        : "API");
  item.dataVolume =
    item.dataVolume ??
    `${(36 + index * 11).toLocaleString("zh-CN")}万行`;
  item.dailyRecords = item.dailyRecords ?? buildAccessDailyRecords(item, index);
});

dataAssets.forEach((item, index) => {
  const profile = assetGovernanceProfiles[item.type] ?? assetGovernanceProfiles["主题数据资产"];
  const blueprint = assetDetailBlueprints[item.name] ?? assetTypeFallbackBlueprints[item.type] ?? {};
  item.id = `asset-${index + 1}`;
  item.ownerDept = item.ownerDept ?? profile.department;
  item.steward = item.steward ?? `资产管理员${index + 1}`;
  item.openLevel = item.openLevel ?? profile.openLevel;
  item.shareStatus = item.shareStatus ?? profile.shareStatus;
  item.serviceMode = item.serviceMode ?? profile.serviceMode;
  item.assetSummary = item.assetSummary ?? blueprint.summary ?? "";
  item.assetIncludes = item.assetIncludes ?? blueprint.includes ?? [];
  item.assetFields = item.assetFields ?? blueprint.fields ?? [];
  item.assetOutputs = item.assetOutputs ?? blueprint.outputs ?? [];
  item.qualityRule =
    item.qualityRule ??
    (item.type === "主数据资产"
      ? "统一名称、统一编码、增量变更校验"
      : item.type === "指标资产"
        ? "口径一致性校验、发布时间校验"
        : "完整性校验、时间新鲜度校验、异常波动校验");
});

metrics.forEach((item, index) => {
  item.id = `metric-${index + 1}`;
});

serviceCatalog.forEach((item, index) => {
  item.id = `svc-${index + 1}`;
  item.ownerDept = item.ownerDept ?? (item.type === "智能服务" ? "信息中心" : "数据办");
  item.shareStatus = item.shareStatus ?? (item.status === "已开通" ? "可申请使用" : "建设中");
});

const shareTypeOptions = ["全部", "数据指标", "主题数据集", "旅游资源"];
const shareStatusOptions = ["全部", "待责任单位确认", "审核中", "补充材料", "已开通"];
const shareMethodOptions = ["接口推送", "表下载", "表下载 + 接口推送"];
const secrecyLevelOptions = ["公开", "内部", "敏感"];

function inferShareTypeFromService(service) {
  const name = service?.name ?? "";
  if (name.includes("资源")) return "旅游资源";
  if (name.includes("数据集")) return "主题数据集";
  return "数据指标";
}

function inferShareMethodFromService(service) {
  const name = service?.name ?? "";
  if (name.includes("主数据")) return "表下载 + 接口推送";
  if (name.includes("API") || name.includes("接口") || name.includes("服务")) return "接口推送";
  return "表下载";
}

function defaultDataDescription(service) {
  const type = inferShareTypeFromService(service);
  if (type === "旅游资源") {
    return `围绕${service?.name ?? "旅游资源共享服务"}提供标准名称、编码、分类、区位、服务属性和关联关系等资源共享数据。`;
  }
  if (type === "主题数据集") {
    return `围绕${service?.name ?? "主题数据集"}提供按业务主题组织的明细、汇总和趋势数据，支持专题分析和运行监测。`;
  }
  return `围绕${service?.name ?? "指标服务"}提供统一指标名称、口径、结果值和时间维度数据，便于驾驶舱和专题分析直接复用。`;
}

function defaultScopeDescription(service) {
  return `面向${service?.target ?? "厅机关、处室、市州区县和景区单位"}提供共享使用，支撑业务查询、专题分析、驾驶舱应用和跨部门协同。`;
}

function buildShareRequestDraft(service) {
  const targetService = service ?? serviceCatalog[0];
  return {
    serviceId: targetService.id,
    serviceName: targetService.name,
    shareType: inferShareTypeFromService(targetService),
    dataDescription: defaultDataDescription(targetService),
    scopeDescription: defaultScopeDescription(targetService),
    shareMethod: inferShareMethodFromService(targetService),
    shareFrequency: targetService.refresh,
    secrecyLevel: "内部",
    ownerDept: targetService.ownerDept,
    applyUnit: "省文化和旅游厅办公室",
    applicant: "张敏",
    applyTime: "2026-07-09 10:30",
  };
}

const shareApplications = ref([
  {
    id: "share-1",
    applicationNo: "GZWL-GX-202607-001",
    serviceId: "svc-2",
    serviceName: "景区客流专题数据集",
    shareType: "主题数据集",
    dataDescription: "共享重点景区发布客流、实时在园人数、日累计入园人数、分时客流趋势等主题数据。",
    scopeDescription: "用于省厅假日值班、全域客流监测、市州节假日分析和重点景区承载预警。",
    shareMethod: "接口推送",
    shareFrequency: "5分钟",
    secrecyLevel: "内部",
    ownerDept: "数据办",
    applyUnit: "省文化和旅游厅办公室",
    applicant: "张敏",
    applyTime: "2026-07-02 10:20",
    status: "已开通",
    currentStep: "共享已开通",
    currentReviewer: "信息中心",
    usageScene: "中秋国庆假日专题监测",
    reviewTimeline: [
      { step: "提交申请", time: "2026-07-02 10:20", status: "已完成", detail: "申请单位提交景区客流专题共享申请单。" },
      { step: "责任单位确认", time: "2026-07-02 14:30", status: "已完成", detail: "数据办确认共享范围、字段口径和使用边界。" },
      { step: "数据办审核", time: "2026-07-03 09:10", status: "已完成", detail: "审核通过，纳入共享开通清单。" },
      { step: "共享开通", time: "2026-07-03 16:00", status: "已完成", detail: "完成接口授权、调用凭证发放和留痕登记。" },
    ],
  },
  {
    id: "share-2",
    applicationNo: "GZWL-GX-202607-002",
    serviceId: "svc-10",
    serviceName: "统一指标查询服务",
    shareType: "数据指标",
    dataDescription: "共享全域客流、住宿运行、消费热度、安全应急等统一口径指标结果和指标说明。",
    scopeDescription: "用于市州文旅单位开展指标查询、横向对比和专题报送。",
    shareMethod: "接口推送",
    shareFrequency: "实时",
    secrecyLevel: "内部",
    ownerDept: "数据办",
    applyUnit: "黔南州文化和旅游局",
    applicant: "李航",
    applyTime: "2026-07-04 11:05",
    status: "审核中",
    currentStep: "数据办审核",
    currentReviewer: "数据办",
    usageScene: "市州运行监测与专题报表",
    reviewTimeline: [
      { step: "提交申请", time: "2026-07-04 11:05", status: "已完成", detail: "申请单位提交统一指标共享申请单。" },
      { step: "责任单位确认", time: "2026-07-04 15:20", status: "已完成", detail: "指标中心确认共享指标范围和口径说明。" },
      { step: "数据办审核", time: "待处理", status: "进行中", detail: "等待数据办完成授权范围与审计策略审核。" },
      { step: "共享开通", time: "未开始", status: "未开始", detail: "审核通过后自动生成调用凭证和接口权限。" },
    ],
  },
  {
    id: "share-3",
    applicationNo: "GZWL-GX-202607-003",
    serviceId: "svc-1",
    serviceName: "旅游资源主数据查询 API",
    shareType: "旅游资源",
    dataDescription: "共享景区、街区、度假区、文化场馆、酒店民宿等旅游资源主数据及资源关联关系。",
    scopeDescription: "用于安顺市资源普查核校、资源底图更新和专题库补数。",
    shareMethod: "表下载 + 接口推送",
    shareFrequency: "实时",
    secrecyLevel: "内部",
    ownerDept: "数据办",
    applyUnit: "安顺市文化广电旅游局",
    applicant: "周洁",
    applyTime: "2026-07-05 09:40",
    status: "补充材料",
    currentStep: "补充材料",
    currentReviewer: "资源开发处",
    usageScene: "资源普查复核与资源底图维护",
    reviewTimeline: [
      { step: "提交申请", time: "2026-07-05 09:40", status: "已完成", detail: "申请单位提交旅游资源主数据共享申请单。" },
      { step: "责任单位确认", time: "2026-07-05 14:15", status: "进行中", detail: "需补充资源使用边界和落库方式说明。" },
      { step: "数据办审核", time: "未开始", status: "未开始", detail: "责任单位确认后进入数据办审核。" },
      { step: "共享开通", time: "未开始", status: "未开始", detail: "审核通过后开通目录下载和接口权限。" },
    ],
  },
  {
    id: "share-4",
    applicationNo: "GZWL-GX-202607-004",
    serviceId: "svc-4",
    serviceName: "文旅消费热度服务",
    shareType: "数据指标",
    dataDescription: "共享消费热度指数、消费笔数、客单价、节假日波动等消费指标结果。",
    scopeDescription: "用于活动传播评估、消费复盘和领导专题汇报。",
    shareMethod: "表下载",
    shareFrequency: "每日",
    secrecyLevel: "内部",
    ownerDept: "数据办",
    applyUnit: "贵阳市文化和旅游局",
    applicant: "王晨",
    applyTime: "2026-07-03 16:25",
    status: "已开通",
    currentStep: "共享已开通",
    currentReviewer: "信息中心",
    usageScene: "贵阳市文旅消费专题分析",
    reviewTimeline: [
      { step: "提交申请", time: "2026-07-03 16:25", status: "已完成", detail: "申请单位提交消费热度共享申请单。" },
      { step: "责任单位确认", time: "2026-07-03 17:10", status: "已完成", detail: "产业发展处确认指标范围与时间粒度。" },
      { step: "数据办审核", time: "2026-07-04 09:00", status: "已完成", detail: "数据办审核通过并登记共享范围。" },
      { step: "共享开通", time: "2026-07-04 12:00", status: "已完成", detail: "已开放日报下载，纳入共享日志留痕。" },
    ],
  },
]);

const sharedDataCatalog = ref([
  {
    id: "shared-1",
    requestId: "share-1",
    requestNo: "GZWL-GX-202607-001",
    serviceId: "svc-2",
    serviceName: "景区客流专题数据集",
    dataName: "重点景区客流五分钟数据",
    tableName: "dws_scenic_flow_5m",
    shareMethod: "接口推送",
    rowCount: "268.4万行",
    status: "已开通",
    openedTime: "2026-07-03 16:00",
    interfaceDetail: {
      endpoint: "/api/share/scenic/flow/v1/push",
      method: "POST",
      auth: "Token + IP 白名单",
      request: "按景区编码、统计时间窗口推送实时入园、出园、在园、累计入园等字段。",
      response: "返回批次编号、接收结果、失败原因和重推标记。",
    },
    downloadDetail: "不提供表下载，按 5 分钟增量接口推送。",
  },
  {
    id: "shared-2",
    requestId: "share-3",
    requestNo: "GZWL-GX-202607-003",
    serviceId: "svc-1",
    serviceName: "旅游资源主数据查询 API",
    dataName: "旅游资源主数据目录表",
    tableName: "mdm_tourism_resource_base",
    shareMethod: "表下载",
    rowCount: "41.2万行",
    status: "待开通",
    openedTime: "待审核通过",
    interfaceDetail: {
      endpoint: "未开通",
      method: "-",
      auth: "-",
      request: "当前申请尚未开通接口推送。",
      response: "审核通过后可生成对应接口清单。",
    },
    downloadDetail: "支持 CSV / XLSX 下载，字段包含资源编码、标准名称、分类、等级、区位、开放信息等。",
  },
  {
    id: "shared-3",
    requestId: "share-4",
    requestNo: "GZWL-GX-202607-004",
    serviceId: "svc-4",
    serviceName: "文旅消费热度服务",
    dataName: "消费热度日报表",
    tableName: "ads_tourism_consume_heat_daily",
    shareMethod: "表下载",
    rowCount: "18.6万行",
    status: "已开通",
    openedTime: "2026-07-04 12:00",
    interfaceDetail: {
      endpoint: "不适用",
      method: "-",
      auth: "-",
      request: "当前共享方式为表下载。",
      response: "可在共享门户按天下载标准报表。",
    },
    downloadDetail: "支持日报表下载，包含城市、日期、消费热度指数、消费笔数、客单价等字段。",
  },
]);

const sharedDataCallCatalog = ref([
  {
    id: "call-1",
    sharedDataId: "shared-1",
    requestNo: "GZWL-GX-202607-001",
    dataName: "重点景区客流五分钟数据",
    callerUnit: "省文化和旅游厅办公室",
    callStatus: "成功",
    callStartTime: "2026-07-11 08:00:03",
    callEndTime: "2026-07-11 08:00:18",
    callMethod: "API接口调用",
    sharedVolume: "12.8万行",
    serviceName: "景区客流专题数据集",
  },
  {
    id: "call-2",
    sharedDataId: "shared-1",
    requestNo: "GZWL-GX-202607-001",
    dataName: "重点景区客流五分钟数据",
    callerUnit: "省文化和旅游厅值班室",
    callStatus: "成功",
    callStartTime: "2026-07-11 09:05:10",
    callEndTime: "2026-07-11 09:05:26",
    callMethod: "API接口调用",
    sharedVolume: "13.4万行",
    serviceName: "景区客流专题数据集",
  },
  {
    id: "call-3",
    sharedDataId: "shared-3",
    requestNo: "GZWL-GX-202607-004",
    dataName: "消费热度日报表",
    callerUnit: "贵阳市文化和旅游局",
    callStatus: "成功",
    callStartTime: "2026-07-11 07:30:00",
    callEndTime: "2026-07-11 07:31:12",
    callMethod: "批量下载",
    sharedVolume: "2.6万行",
    serviceName: "文旅消费热度服务",
  },
  {
    id: "call-4",
    sharedDataId: "shared-2",
    requestNo: "GZWL-GX-202607-003",
    dataName: "旅游资源主数据目录表",
    callerUnit: "安顺市文化广电旅游局",
    callStatus: "待开通",
    callStartTime: "未开始",
    callEndTime: "未开始",
    callMethod: "目录下载",
    sharedVolume: "0行",
    serviceName: "旅游资源主数据查询 API",
  },
  {
    id: "call-5",
    sharedDataId: "shared-3",
    requestNo: "GZWL-GX-202607-004",
    dataName: "消费热度日报表",
    callerUnit: "贵阳市文化和旅游局",
    callStatus: "失败",
    callStartTime: "2026-07-10 23:30:00",
    callEndTime: "2026-07-10 23:30:07",
    callMethod: "批量下载",
    sharedVolume: "0行",
    serviceName: "文旅消费热度服务",
  },
]);

const shareRequestForm = reactive(buildShareRequestDraft(serviceCatalog[1] ?? serviceCatalog[0]));

const permissionMenus = [
  {
    role: "厅机关管理员",
    scope: "全省",
    menus: ["总览驾驶舱", "资源管理", "业务系统目录", "数据资产中心", "数据接入与共享", "权限管理", "治理与架构"],
    dataAccess: "全省资源明细、业务系统清单、全量资产目录、共享审批与权限配置。",
  },
  {
    role: "处室业务用户",
    scope: "按条线",
    menus: ["总览驾驶舱", "资源管理", "业务系统目录", "数据资产中心", "数据接入与共享"],
    dataAccess: "本条线资源、资产、指标和服务目录可查，可发起共享申请。",
  },
  {
    role: "市州文旅单位",
    scope: "辖区",
    menus: ["总览驾驶舱", "资源管理", "数据资产中心", "数据接入与共享"],
    dataAccess: "辖区资源和授权后的主题资产、指标及服务可见。",
  },
  {
    role: "景区运营单位",
    scope: "本单位",
    menus: ["资源管理", "数据资产中心", "数据接入与共享"],
    dataAccess: "本单位基础档案、运营指标和已授权的接口服务可查。",
  },
];

const permissionRules = [
  { type: "功能权限", object: "菜单与页面", sample: "是否可见数据资产中心、是否可配置权限管理", policy: "按角色授权到页面、按钮和操作动作。" },
  { type: "数据权限", object: "资源范围", sample: "全省 / 市州 / 区县 / 本单位", policy: "按行政区划、所属单位、资源分类和业务条线进行控制。" },
  { type: "字段权限", object: "敏感字段", sample: "联系方式、入住明细、证件号码", policy: "按脱敏规则和字段等级开放。" },
  { type: "服务权限", object: "接口与数据集", sample: "API 调用、专题数据集下载、智能问数", policy: "按审批结果、调用凭证和审计留痕统一开通。" },
];

const permissionModules = [
  { id: "users", label: "用户管理", icon: Users },
  { id: "roles", label: "角色管理", icon: UserCog },
  { id: "orgs", label: "组织机构管理", icon: Building2 },
];

const permissionFunctionOptions = [
  "总览驾驶舱",
  "资源管理",
  "业务系统目录",
  "数据资产中心",
  "数据接入与共享",
  "数据看板",
  "权限管理",
  "治理与架构",
];

const permissionDataOptions = [
  "全省资源明细",
  "辖区资源明细",
  "数据资产明细",
  "指标口径与趋势",
  "接入数据明细",
  "数据共享服务调用",
  "敏感字段脱敏查看",
];

const userDirectory = reactive([
  {
    id: "user-1",
    name: "张敏",
    account: "zhangmin",
    org: "省文化和旅游厅办公室",
    role: "厅机关管理员",
    status: "启用",
    scope: "全省",
    password: "123456",
    loginState: "在线",
    lastLogin: "2026-07-09 09:18",
    currentSession: "当前会话",
    loginIp: "10.25.1.18",
    device: "Windows 办公终端",
  },
  {
    id: "user-2",
    name: "王超",
    account: "wangchao",
    org: "市场管理处",
    role: "处室业务用户",
    status: "启用",
    scope: "按条线",
    password: "123456",
    loginState: "在线",
    lastLogin: "2026-07-09 08:47",
    currentSession: "活跃会话",
    loginIp: "10.25.3.42",
    device: "Windows 办公终端",
  },
  {
    id: "user-3",
    name: "刘琴",
    account: "liuqin",
    org: "贵阳市文化和旅游局",
    role: "市州文旅单位",
    status: "启用",
    scope: "贵阳市",
    password: "123456",
    loginState: "离线",
    lastLogin: "2026-07-08 18:26",
    currentSession: "无在线会话",
    loginIp: "10.31.6.15",
    device: "政务外网终端",
  },
  {
    id: "user-4",
    name: "陈航",
    account: "chenhang",
    org: "黄果树景区运营中心",
    role: "景区运营单位",
    status: "启用",
    scope: "本单位",
    password: "123456",
    loginState: "在线",
    lastLogin: "2026-07-09 07:58",
    currentSession: "活跃会话",
    loginIp: "10.48.2.31",
    device: "景区值班终端",
  },
  {
    id: "user-5",
    name: "杨静",
    account: "yangjing",
    org: "信息中心",
    role: "厅机关管理员",
    status: "停用",
    scope: "全省",
    password: "123456",
    loginState: "停用",
    lastLogin: "2026-07-03 16:20",
    currentSession: "已禁用",
    loginIp: "10.25.4.66",
    device: "运维工作站",
  },
]);

const roleDirectory = reactive([
  {
    id: "role-1",
    name: "厅机关管理员",
    memberCount: 18,
    orgScope: "全省",
    functionModules: ["总览驾驶舱", "资源管理", "业务系统目录", "数据资产中心", "数据接入与共享", "数据看板", "权限管理", "治理与架构"],
    dataPermissions: ["全省资源明细", "数据资产明细", "指标口径与趋势", "接入数据明细", "数据共享服务调用", "敏感字段脱敏查看"],
    fieldPolicy: "按字段等级脱敏开放",
  },
  {
    id: "role-2",
    name: "处室业务用户",
    memberCount: 42,
    orgScope: "按条线",
    functionModules: ["总览驾驶舱", "资源管理", "业务系统目录", "数据资产中心", "数据接入与共享", "数据看板"],
    dataPermissions: ["辖区资源明细", "数据资产明细", "指标口径与趋势", "数据共享服务调用"],
    fieldPolicy: "默认脱敏，审批后扩展",
  },
  {
    id: "role-3",
    name: "市州文旅单位",
    memberCount: 27,
    orgScope: "辖区",
    functionModules: ["总览驾驶舱", "资源管理", "数据资产中心", "数据接入与共享", "数据看板"],
    dataPermissions: ["辖区资源明细", "指标口径与趋势", "数据共享服务调用"],
    fieldPolicy: "辖区内数据开放",
  },
  {
    id: "role-4",
    name: "景区运营单位",
    memberCount: 36,
    orgScope: "本单位",
    functionModules: ["资源管理", "数据资产中心", "数据接入与共享"],
    dataPermissions: ["辖区资源明细", "数据共享服务调用"],
    fieldPolicy: "本单位敏感字段脱敏开放",
  },
]);

const orgDirectory = reactive([
  { id: "org-1", name: "贵州省文化和旅游厅", level: "省级", parent: "省级根节点", userCount: 68, roleCount: 4, coverage: "全省" },
  { id: "org-2", name: "市场管理处", level: "处室", parent: "贵州省文化和旅游厅", userCount: 12, roleCount: 2, coverage: "市场监管条线" },
  { id: "org-3", name: "信息中心", level: "直属单位", parent: "贵州省文化和旅游厅", userCount: 16, roleCount: 2, coverage: "平台运维与智能应用" },
  { id: "org-4", name: "贵阳市文化和旅游局", level: "市州", parent: "贵州省文化和旅游厅", userCount: 10, roleCount: 1, coverage: "贵阳市" },
  { id: "org-5", name: "黄果树景区运营中心", level: "景区单位", parent: "安顺市文化和旅游局", userCount: 8, roleCount: 1, coverage: "黄果树景区" },
]);

const analysisMetricOptions = [
  { id: "total", label: "总资源数" },
  ...allResourceCategories.map((item) => ({ id: item, label: item })),
];

const analysisModules = [
  { id: "access", label: "数据接入与共享概况" },
  { id: "business", label: "业务看板" },
];

const queryModules = [
  { id: "query-overview", label: "关键指标" },
  { id: "query-explore", label: "多维查询" },
  { id: "query-reports", label: "专题报表" },
  { id: "query-favorites", label: "我的查询" },
];

const businessBoardModules = [{ id: "ruralTourism", label: "乡村旅游" }];
const businessBoardViews = [
  { id: "matrix", label: "多维表" },
  { id: "report", label: "报表" },
];

const guizhouMapViewport = {
  width: 620,
  height: 500,
  padding: 20,
};

const guizhouCityNameMap = {
  黔东南苗族侗族自治州: "黔东南州",
  黔南布依族苗族自治州: "黔南州",
  黔西南布依族苗族自治州: "黔西南州",
};

const guizhouLabelOffsets = {
  毕节市: { x: -8, y: -10 },
  遵义市: { x: 8, y: -8 },
  铜仁市: { x: 8, y: -4 },
  六盘水市: { x: -10, y: 10 },
  安顺市: { x: 0, y: 8 },
  贵阳市: { x: 6, y: 4 },
  黔南州: { x: 8, y: 10 },
  黔东南州: { x: 14, y: 8 },
  黔西南州: { x: -4, y: 8 },
};

function normalizeGuizhouCityName(name) {
  return guizhouCityNameMap[name] ?? name;
}

function geometryToPolygons(geometry) {
  if (!geometry) return [];
  if (geometry.type === "Polygon") return [geometry.coordinates];
  if (geometry.type === "MultiPolygon") return geometry.coordinates;
  return [];
}

function getFeatureCollectionBounds(collection) {
  let minLon = Infinity;
  let maxLon = -Infinity;
  let minLat = Infinity;
  let maxLat = -Infinity;

  for (const feature of collection?.features ?? []) {
    for (const polygon of geometryToPolygons(feature.geometry)) {
      for (const ring of polygon) {
        for (const point of ring) {
          const [lon, lat] = point;
          minLon = Math.min(minLon, lon);
          maxLon = Math.max(maxLon, lon);
          minLat = Math.min(minLat, lat);
          maxLat = Math.max(maxLat, lat);
        }
      }
    }
  }

  return {
    minLon,
    maxLon,
    minLat,
    maxLat,
  };
}

function createGeoProjector(bounds, viewport) {
  const lonSpan = Math.max(bounds.maxLon - bounds.minLon, 0.001);
  const latSpan = Math.max(bounds.maxLat - bounds.minLat, 0.001);
  const scale = Math.min(
    (viewport.width - viewport.padding * 2) / lonSpan,
    (viewport.height - viewport.padding * 2) / latSpan,
  );
  const xOffset = (viewport.width - lonSpan * scale) / 2;
  const yOffset = (viewport.height - latSpan * scale) / 2;

  return ([lon, lat]) => [
    xOffset + (lon - bounds.minLon) * scale,
    viewport.height - yOffset - (lat - bounds.minLat) * scale,
  ];
}

function geometryToSvgPath(geometry, project) {
  return geometryToPolygons(geometry)
    .flatMap((polygon) =>
      polygon
        .filter((ring) => ring.length > 0)
        .map((ring) =>
          ring
            .map(([lon, lat], pointIndex) => {
              const [x, y] = project([lon, lat]);
              return `${pointIndex === 0 ? "M" : "L"}${x.toFixed(2)} ${y.toFixed(2)}`;
            })
            .join(" ") + " Z",
        ),
    )
    .join(" ");
}

const guizhouBounds = getFeatureCollectionBounds(guizhouOutline);
const projectGuizhouPoint = createGeoProjector(guizhouBounds, guizhouMapViewport);

const guizhouProvincePath = (guizhouOutline.features ?? [])
  .map((feature) => geometryToSvgPath(feature.geometry, projectGuizhouPoint))
  .join(" ");

const guizhouMapRegions = (guizhouBoundary.features ?? []).map((feature) => {
  const city = normalizeGuizhouCityName(feature.properties?.name ?? "");
  const centroid = feature.properties?.centroid ?? feature.properties?.center ?? [0, 0];
  const [labelBaseX, labelBaseY] = projectGuizhouPoint(centroid);
  const offset = guizhouLabelOffsets[city] ?? { x: 0, y: 0 };

  return {
    id: String(feature.properties?.adcode ?? city),
    name: city,
    city,
    path: geometryToSvgPath(feature.geometry, projectGuizhouPoint),
    labelX: labelBaseX + offset.x,
    labelY: labelBaseY + offset.y,
  };
});

const accessTrend = [
  { month: "1月", value: 28 },
  { month: "2月", value: 31 },
  { month: "3月", value: 36 },
  { month: "4月", value: 39 },
  { month: "5月", value: 45 },
  { month: "6月", value: 48 },
  { month: "7月", value: 54 },
];

const shareTrend = [
  { month: "1月", value: 16 },
  { month: "2月", value: 19 },
  { month: "3月", value: 22 },
  { month: "4月", value: 26 },
  { month: "5月", value: 29 },
  { month: "6月", value: 33 },
  { month: "7月", value: 38 },
];

const governanceItems = [
  { title: "统一资源管理", desc: "将景区、街区、度假区、乡村旅游点、文化场馆、酒店民宿、旅行社、导游、活动和涉旅经营主体统一纳管。" },
  { title: "统一业务系统目录", desc: "把现有业务系统、外部数据和智能应用按模块分类管理，明确来源、用途、状态和接入优先级。" },
  { title: "统一数据资产中心", desc: "把主数据、主题数据集、指标资产、标签画像、知识库、视频专题和服务资产统一纳管。" },
  { title: "统一指标资产", desc: "将指标说明、算法、来源、责任单位和适用场景沉淀为可复用的数据资产。" },
  { title: "统一服务出口", desc: "通过接口服务、专题数据集、指标服务和智能服务统一供数。" },
  { title: "统一权限审计", desc: "实现目录可见、数据可控、服务可审、共享可追踪。" },
];

const architectureLayers = [
  { title: "接入层", desc: "接入旅游资源、乡村旅游、监管执法、信用监管、活动管理、住宿业、LBS、消费、气象和视频监控等来源。" },
  { title: "治理层", desc: "完成标准化、编码统一、名称治理、质量校验、标签加工、脱敏处理和主数据关联。" },
  { title: "资产层", desc: "形成资源管理、业务系统目录、数据资产中心、知识库、视频专题和指标资产等沉淀。" },
  { title: "服务层", desc: "通过 API、专题数据集、指标服务、视频服务和智能服务统一支撑门户、驾驶舱、智能体和共享交换。" },
];

const serviceRequests = [
  { title: "中秋国庆假期客流专题共享", status: "审核中", unit: "省文化和旅游厅办公室", detail: "已完成责任单位确认，等待数据办审核。" },
  { title: "市州住宿运行专题接口", status: "已开通", unit: "黔南州文化和旅游局", detail: "已授权调用辖区住宿运行专题指标与接口。" },
  { title: "活动传播评估数据申请", status: "补充材料", unit: "贵阳市文化和旅游局", detail: "需补充使用边界与成果产出说明。" },
];

const dashboardCards = computed(() => [
  { label: "今日游客总量", value: "128.6万人次", meta: "较昨日 +8.4%" },
  { label: "实时在园人数", value: "21.4万人", meta: "重点景区实时汇聚" },
  { label: "过夜游客量", value: "32.8万人", meta: "住宿运行专题" },
  { label: "今日旅游收入", value: "6.7亿元", meta: "消费与订单综合测算" },
  { label: "重点景区客流", value: "58.3万人次", meta: "TOP100 景区" },
  { label: "风险预警数", value: "14条", meta: "气象 / 客流 / 停车 / 视频" },
  { label: "已开通共享", value: `${shareApplications.value.filter((item) => item.status === "已开通").length}项`, meta: "共享申请已办结" },
  { label: "智能问数调用", value: "2,416次", meta: "今日累计" },
]);

const dashboardOperationTrend = [
  { month: "1月", visitors: 84, stay: 18, revenue: 4.2 },
  { month: "2月", visitors: 91, stay: 20, revenue: 4.6 },
  { month: "3月", visitors: 96, stay: 22, revenue: 4.9 },
  { month: "4月", visitors: 108, stay: 25, revenue: 5.5 },
  { month: "5月", visitors: 118, stay: 29, revenue: 6.1 },
  { month: "6月", visitors: 123, stay: 31, revenue: 6.4 },
  { month: "7月", visitors: 129, stay: 33, revenue: 6.7 },
];

const dashboardTrendMax = Math.max(...dashboardOperationTrend.map((item) => item.visitors), 1);

const dashboardTopicCards = [
  {
    title: "客流监测",
    eyebrow: "专题一",
    action: "resources",
    metrics: [
      { label: "发布客流", value: "58.3万人次" },
      { label: "在园人数", value: "21.4万人" },
      { label: "超承载景区", value: "3个" },
    ],
    note: "黄果树、梵净山、荔波小七孔客流持续高位运行。",
  },
  {
    title: "住宿运行",
    eyebrow: "专题二",
    action: "assets",
    metrics: [
      { label: "入住率", value: "74.8%" },
      { label: "平均房价", value: "368元" },
      { label: "过夜游客", value: "32.8万人" },
    ],
    note: "贵阳、黔东南、黔南住宿热度靠前，重点县区周末上涨明显。",
  },
  {
    title: "消费运行",
    eyebrow: "专题三",
    action: "metrics",
    metrics: [
      { label: "消费热度", value: "87.2" },
      { label: "消费笔数", value: "96.4万笔" },
      { label: "客单价", value: "143元" },
    ],
    note: "夜游街区与重点活动带动消费增长，贵阳、遵义、黔东南表现突出。",
  },
];

const dashboardSecurityCards = [
  {
    title: "安全预警",
    eyebrow: "风险一览",
    metrics: [
      { label: "气象预警", value: "5条" },
      { label: "客流预警", value: "6条" },
      { label: "停车预警", value: "3条" },
    ],
    note: "黔东南州强降雨蓝色预警持续，3个重点景区停车接近饱和。",
  },
  {
    title: "应急处置",
    eyebrow: "处置闭环",
    metrics: [
      { label: "处理中事件", value: "4件" },
      { label: "今日办结", value: "11件" },
      { label: "闭环率", value: "92.3%" },
    ],
    note: "重点事件已完成会商调度，当前无重大突发事件升级。",
  },
  {
    title: "视频监控",
    eyebrow: "视频在线",
    metrics: [
      { label: "纳管点位", value: "1,286路" },
      { label: "在线率", value: "96.8%" },
      { label: "会商调用", value: "18次" },
    ],
    note: "黄果树、梵净山、西江苗寨重点点位视频均可正常调用。",
  },
];

const dashboardFoundationCards = [
  {
    title: "数据底座成效",
    eyebrow: "建设产出",
    metrics: [
      { label: "接入数据源", value: `${dataAccessCatalog.length}项` },
      { label: "沉淀数据资产", value: `${dataAssets.length}项` },
      { label: "统一指标", value: `${metrics.length}项` },
      { label: "开通共享", value: `${shareApplications.value.filter((item) => item.status === "已开通").length}项` },
    ],
    note: "景区名称、主体编码、指标口径已统一，驾驶舱与智能应用共用同一底座。",
  },
  {
    title: "智能应用运行",
    eyebrow: "AI 支撑",
    metrics: [
      { label: "政府智能体", value: "826次" },
      { label: "黄小西智能体", value: "1,143次" },
      { label: "酒店智能体", value: "447次" },
      { label: "智能报告", value: "36份" },
    ],
    note: "智能问数、景区伴游和酒店问答均已接入底座统一数据与知识能力。",
  },
];

const dashboardMonitorTaskSummary = computed(() => {
  const successCount = dataAccessCatalog.reduce((sum, item) => sum + item.dailyRecords.filter((record) => record.taskStatus === "成功").length, 0);
  const runningCount = dataAccessCatalog.reduce((sum, item) => sum + item.dailyRecords.filter((record) => record.taskStatus === "进行中").length, 0);
  const totalTasks = successCount + runningCount + 1286;
  const recentTotal = successCount + runningCount;
  const recentFailed = 2;
  const recentSuccess = Math.max(0, recentTotal - recentFailed);
  const successRate = recentTotal ? ((recentSuccess / recentTotal) * 100).toFixed(1) : "0.0";

  return {
    totalTasks,
    recentSuccess,
    recentFailed,
    successRate,
  };
});

const dashboardMonitorTaskCards = computed(() => [
  {
    label: "累计采集任务",
    value: `${dashboardMonitorTaskSummary.value.totalTasks.toLocaleString("zh-CN")}次`,
    meta: "平台累计执行",
  },
  {
    label: "近24小时成功",
    value: `${dashboardMonitorTaskSummary.value.recentSuccess}次`,
    meta: "采集任务成功次数",
  },
  {
    label: "近24小时失败",
    value: `${dashboardMonitorTaskSummary.value.recentFailed}次`,
    meta: "采集任务失败次数",
  },
  {
    label: "近24小时成功率",
    value: `${dashboardMonitorTaskSummary.value.successRate}%`,
    meta: "成功次数 / 总次数",
  },
]);

const dashboardTodayTaskDetails = computed(() =>
  dataAccessCatalog.slice(0, 8).map((item, index) => {
    const record = item.dailyRecords[0];
    return {
      taskName: `${item.name}采集任务`,
      status: index === 5 ? "失败" : record.taskStatus,
      startTime: record.taskStartTime,
      endTime: index === 5 ? "2026-07-11 03:18" : record.taskEndTime,
      duration: record.taskEndTime === "执行中" ? "执行中" : index === 5 ? "8分钟" : `${24 + index * 3}分钟`,
    };
  }),
);

const dashboardSharedCallSuccess = computed(() => sharedDataCallCatalog.value.filter((item) => item.callStatus === "成功").length);
const dashboardSharedCallFailed = computed(() => sharedDataCallCatalog.value.filter((item) => item.callStatus === "失败").length);
const dashboardSharedCallTotal = computed(() => sharedDataCallCatalog.value.length);
const dashboardSharedCallSuccessRate = computed(() =>
  dashboardSharedCallTotal.value ? Math.round((dashboardSharedCallSuccess.value / dashboardSharedCallTotal.value) * 100) : 0,
);
const dashboardShareMonitorCards = computed(() => [
  { label: "已发布服务接口", value: `${serviceCatalog.length}个`, meta: "服务总数统计" },
  { label: "累计服务调用", value: "18,642次", meta: "服务调用统计" },
  { label: "今日调用成功", value: `${dashboardSharedCallSuccess.value}次`, meta: "调用状态监控" },
  { label: "今日调用失败", value: `${dashboardSharedCallFailed.value}次`, meta: "失败调用记录" },
]);

const dashboardTodoItems = [
  {
    title: "共享申请待审核",
    category: "数据共享",
    status: "审核中",
    detail: "统一指标查询服务共享申请已进入数据办审核环节，需完成用途核验和开通确认。",
    unit: "数据办",
    coord: "黔南州文化和旅游局",
    deadline: "今日 17:00 前",
    actionLabel: "查看审核",
    targetType: "share-review-detail",
    targetId: "share-1",
  },
  {
    title: "住宿专题接入补数",
    category: "数据接入",
    status: "进行中",
    detail: "PMS 入住经营数据仍有 12 家重点酒店待补齐历史数据，需尽快完成回溯同步。",
    unit: "产业发展处",
    coord: "信息中心",
    deadline: "本周内完成",
    actionLabel: "查看接入",
    targetType: "access-detail",
    targetId: "access-2",
  },
  {
    title: "驾驶舱指标重构",
    category: "指标治理",
    status: "待推进",
    detail: "现有 18 个驾驶舱指标需从单表口径切换到统一指标中心，形成复用指标资产。",
    unit: "数据办",
    coord: "各业务处室",
    deadline: "7 月中旬前",
    actionLabel: "查看指标",
    targetType: "metric-detail",
    targetId: "metric-1",
  },
];

const dashboardWarningItems = [
  {
    title: "黄果树景区承载预警",
    category: "客流承载",
    status: "预警",
    detail: "实时在园人数达到最大承载量的 87%，建议立即启动分时预约提示和现场分流。",
    unit: "安顺市文旅局",
    coord: "黄果树景区",
    trigger: "实时触发",
    actionLabel: "查看景区",
    targetType: "resource-detail",
    targetId: "scenic-1",
  },
  {
    title: "西江苗寨停车饱和",
    category: "停车调度",
    status: "预警",
    detail: "停车场剩余车位不足 8%，已触发停车引导提醒，建议同步发布周边分流信息。",
    unit: "黔东南州文旅局",
    coord: "西江景区运营方",
    trigger: "5 分钟更新",
    actionLabel: "查看景区",
    targetType: "resource-detail",
    targetId: "village-1",
  },
  {
    title: "黔东南强降雨风险",
    category: "气象风险",
    status: "关注",
    detail: "未来 6 小时有短时强降雨，需重点关注山地景区步道安全、视频巡检和游客提醒。",
    unit: "安全应急处",
    coord: "黔东南州文旅局",
    trigger: "气象预警推送",
    actionLabel: "查看接入",
    targetType: "access-detail",
    targetId: "access-10",
  },
];

const dashboardMessageCards = computed(() => [
  {
    id: "dashboard-todos",
    eyebrow: "待办提醒",
    title: "事项办理清单",
    priority: "常规跟进",
    count: `${dashboardTodoItems.length} 项`,
    summary: `当前共有 ${dashboardTodoItems.length} 项待办，涉及数据共享、数据接入和指标治理。`,
    leadTitle: dashboardTodoItems[0]?.title ?? "暂无待办事项",
    leadMeta: dashboardTodoItems[0] ? `${dashboardTodoItems[0].unit} · ${dashboardTodoItems[0].deadline}` : "当前无待办",
    detail: dashboardTodoItems[0]?.detail ?? "当前暂无待办任务。",
    icon: CalendarDays,
    tone: "todo",
    actionText: "进入办理",
  },
  {
    id: "dashboard-warnings",
    eyebrow: "预警提示",
    title: "风险与异常处置",
    priority: "高优先级",
    count: `${dashboardWarningItems.length} 项`,
    summary: `当前共有 ${dashboardWarningItems.filter((item) => item.status === "预警").length} 条预警、${dashboardWarningItems.filter((item) => item.status === "关注").length} 条关注事项。`,
    leadTitle: dashboardWarningItems[0]?.title ?? "暂无预警提示",
    leadMeta: dashboardWarningItems[0] ? `${dashboardWarningItems[0].unit} · ${dashboardWarningItems[0].trigger}` : "当前无预警",
    detail: dashboardWarningItems[0]?.detail ?? "当前暂无预警信息。",
    icon: ShieldCheck,
    tone: "warning",
    actionText: "优先查看",
  },
]);

const dashboardModules = computed(() => [
  { title: "资源管理", value: `${resources.length} 条`, view: "resources", icon: FolderTree },
  { title: "业务系统目录", value: `${businessSystems.filter((item) => item.type === "业务系统").length} 项`, view: "systems", icon: Building2 },
  { title: "数据资产中心", value: `${dataAssets.length + metrics.length} 项`, view: "assets", icon: Database },
  { title: "数据查询中心", value: `${queryMetricCatalog.length} 项`, view: "query", icon: Search },
  { title: "数据接入与共享", value: `${dataAccessCatalog.length + serviceCatalog.length} 项`, view: "services", icon: Send },
]);

const defaultQueryConfig = {
  theme: "flow",
  metricId: "lbs_flow",
  timePreset: "month-2026-07",
  granularity: "日",
  region: "全省",
  dimension: "市州",
  source: "综合口径",
  resourceCategory: "全部",
  levelFilter: "全部",
  chargeFilter: "全部",
};

const appState = reactive({
  isAuthenticated: false,
  view: "dashboard",
  query: "",
  metricQuery: "",
  queryModule: "query-overview",
  queryTheme: defaultQueryConfig.theme,
  queryMetricId: defaultQueryConfig.metricId,
  queryTimePreset: defaultQueryConfig.timePreset,
  queryGranularity: defaultQueryConfig.granularity,
  queryRegion: defaultQueryConfig.region,
  queryDimension: defaultQueryConfig.dimension,
  querySource: defaultQueryConfig.source,
  queryResourceCategory: defaultQueryConfig.resourceCategory,
  queryLevelFilter: defaultQueryConfig.levelFilter,
  queryChargeFilter: defaultQueryConfig.chargeFilter,
  queryExecuted: { ...defaultQueryConfig },
  queryResultView: "trend",
  metricDomain: "全部",
  resourceGroup: "tourism-resources",
  resourceCategory: "景区",
  selectedResourceId: "scenic-1",
  systemType: "全部",
  assetType: "全部",
  assetModule: "asset-overview",
  serviceType: "全部",
  shareTypeFilter: "全部",
  shareStatusFilter: "全部",
  shareModule: "access",
  shareTertiary: "share-list",
  accessTypeFilter: "全部",
  accessStatusFilter: "全部",
  accessKeyword: "",
  shareKeyword: "",
  shareReviewStatusFilter: "全部",
  shareReviewStepFilter: "全部",
  shareReviewKeyword: "",
  sharedDataMethodFilter: "全部",
  sharedDataStatusFilter: "全部",
  sharedDataKeyword: "",
  sharedCallMethodFilter: "全部",
  sharedCallStatusFilter: "全部",
  sharedCallKeyword: "",
  servicesPageSize: 10,
  accessPage: 1,
  shareListPage: 1,
  shareReviewPage: 1,
  sharedDataPage: 1,
  sharedCallPage: 1,
  permissionModule: "users",
  analysisModule: "access",
  businessBoardModule: "ruralTourism",
  businessBoardView: "matrix",
  analysisMetric: "total",
  selectedSystemId: "sys-1",
  selectedAssetId: "asset-1",
  selectedMetricId: "metric-1",
  selectedServiceId: "svc-1",
  selectedAccessId: "access-1",
  selectedShareApplicationId: "share-1",
  selectedSharedDataId: "shared-1",
  currentUserId: "user-1",
  selectedUserId: "user-1",
  selectedRoleId: "role-1",
  selectedOrgId: "org-1",
});

const loginForm = reactive({
  account: "zhangmin",
  password: "123456",
});

const loginMessage = ref("请选择可用账号登录系统。");

const viewMetaMap = {
  dashboard: {
    eyebrow: "全省文旅运行总览",
    description: "汇总客流、住宿、消费、安全、应急与智能应用运行情况，支撑领导总览和调度研判。",
    tag: "领导驾驶舱",
  },
  "dashboard-todos": {
    label: "待办清单",
    eyebrow: "驾驶舱消息中心",
    description: "集中查看当前待推进事项、责任单位、协同对象和办理时限，支撑事项督办。",
    tag: "驾驶舱",
  },
  "dashboard-warnings": {
    label: "预警提示",
    eyebrow: "驾驶舱消息中心",
    description: "集中查看客流、停车、气象等风险预警与处置提示，支撑快速研判与响应。",
    tag: "驾驶舱",
  },
  "dashboard-monitor": {
    label: "运行监控",
    eyebrow: "平台运行监控",
    description: "汇总资源地图、采集任务执行、数据服务发布和调用状态，支撑平台运行值守。",
    tag: "驾驶舱",
  },
  resources: {
    eyebrow: "资源统一管理",
    description: "统一查看景区、街区、度假区、文化场馆、酒店民宿等资源目录及其关联数据。",
    tag: "资源管理",
  },
  systems: {
    eyebrow: "业务系统纳管",
    description: "集中呈现厅内业务系统、责任处室和访问入口，支撑系统统管统查。",
    tag: "系统目录",
  },
  assets: {
    eyebrow: "数据资产统一管理",
    description: "统一纳管主数据、主题数据集、指标资产、标签画像、知识库和服务资产，支撑查询、共享和智能应用复用。",
    tag: "资产中心",
  },
  query: {
    eyebrow: "指标与数据自助查询",
    description: "围绕关键指标、资源明细和专题数据集提供多维查询、透视分析、专题报表和常用查询能力。",
    tag: "数据查询中心",
  },
  services: {
    eyebrow: "接入与共享协同",
    description: "覆盖数据接入、共享申请、审核开通和共享清单全流程，形成可追踪的服务链路。",
    tag: "数据接入与共享",
  },
  analysis: {
    eyebrow: "业务分析看板",
    description: "围绕资源分布、接入共享和重点业务主题提供可视化分析，支撑业务研判与专题汇报。",
    tag: "分析看板",
  },
  permissions: {
    eyebrow: "权限体系配置",
    description: "围绕用户、角色和组织机构配置功能权限与数据权限，支撑分级分域使用。",
    tag: "权限管理",
  },
  governance: {
    eyebrow: "治理与架构",
    description: "统一呈现数据底座建设路径、标准体系和技术架构，支撑立项汇报与建设统筹。",
    tag: "建设架构",
  },
  "resource-detail": {
    label: "资源详情",
    eyebrow: "资源目录详情",
    description: "查看单体资源基础信息、补充属性和已关联数据内容。",
    tag: "详情页",
  },
  "asset-detail": {
    label: "资产详情",
    eyebrow: "数据资产详情",
    description: "查看资产定义、分层内容、关联资源和可复用场景。",
    tag: "详情页",
  },
  "metric-detail": {
    label: "指标资产详情",
    eyebrow: "指标资产详情",
    description: "查看指标口径、计算规则、来源资产及应用场景。",
    tag: "详情页",
  },
  "access-detail": {
    label: "接入详情",
    eyebrow: "数据接入详情",
    description: "查看接入方式、数据量、来源系统和当前接入状态。",
    tag: "详情页",
  },
  "service-detail": {
    label: "服务详情",
    eyebrow: "共享服务详情",
    description: "查看服务内容、责任单位、开通状态和可申请共享范围。",
    tag: "详情页",
  },
  "share-detail": {
    label: "共享详情",
    eyebrow: "共享申请详情",
    description: "查看申请信息、审核环节、关联资产和共享开通情况。",
    tag: "流程页",
  },
  "share-create": {
    label: "新建共享申请",
    eyebrow: "共享申请发起",
    description: "配置共享内容、使用范围和共享方式，提交进入审核流程。",
    tag: "流程页",
  },
  "share-review": {
    label: "共享审核",
    eyebrow: "共享审核管理",
    description: "统一查看共享申请审核进度、状态分布和待办事项。",
    tag: "流程页",
  },
  "share-review-detail": {
    label: "审核详情",
    eyebrow: "审核过程详情",
    description: "查看审核意见、处理节点和最终开通结论。",
    tag: "流程页",
  },
  "shared-data": {
    label: "共享清单",
    eyebrow: "共享结果管理",
    description: "查看审核通过后已开通共享的清单、方式和关联申请单。",
    tag: "流程页",
  },
  "shared-data-detail": {
    label: "共享清单详情",
    eyebrow: "共享清单详情",
    description: "查看共享表、接口方式、共享条数和具体服务信息。",
    tag: "详情页",
  },
};

const selectedNav = computed(() => {
  const navItem = navigation.find((item) => item.id === currentPrimaryNavId.value);
  const meta = viewMetaMap[appState.view] ?? {};
  const fallback = navigation[0];
  const secondaryLabel = secondaryNavigation.value.find((item) => item.id === activeSecondaryNavId.value)?.label;
  const shouldUseSecondaryLabel =
    [
      "dashboard",
      "resources",
      "systems",
      "assets",
      "query",
      "services",
      "analysis",
      "permissions",
      "governance",
    ].includes(appState.view) && !!secondaryLabel;
  return {
    ...(navItem ?? fallback),
    ...meta,
    label: meta.label ?? (shouldUseSecondaryLabel ? secondaryLabel : navItem?.label ?? fallback.label),
    tag: meta.tag ?? (shouldUseSecondaryLabel ? navItem?.label ?? fallback.label : undefined),
  };
});

const currentPrimaryNavId = computed(() => viewParentMap[appState.view] ?? appState.view);

const activeResourceGroup = computed(
  () => resourceGroups.find((item) => item.id === appState.resourceGroup) ?? resourceGroups[0],
);

const resourceCategories = computed(() => activeResourceGroup.value.categories);

function resourceGroupIdByCategory(category) {
  return resourceGroups.find((item) => item.categories.includes(category))?.id ?? resourceGroups[0].id;
}

function secondaryNavigationFor(primaryId) {
  switch (primaryId) {
    case "dashboard":
      return [
        { id: "dashboard-home", label: "运行总览" },
        { id: "dashboard-monitor", label: "运行监控" },
        { id: "dashboard-todos", label: "待办清单" },
        { id: "dashboard-warnings", label: "预警提示" },
      ];
    case "resources":
      return resourceGroups.map((item) => ({ id: item.id, label: item.label }));
    case "systems":
      return [{ id: "systems-directory", label: "系统清单" }];
    case "assets":
      return assetNavigationItems;
    case "query":
      return queryModules;
    case "services":
      return serviceNavigationItems;
    case "analysis":
      return analysisModules;
    case "permissions":
      return permissionModules;
    case "governance":
      return [{ id: "governance-overview", label: "建设总览" }];
    default:
      return [];
  }
}

const secondaryNavigation = computed(() => secondaryNavigationFor(currentPrimaryNavId.value));

const activeSecondaryNavId = computed(() => {
  switch (currentPrimaryNavId.value) {
    case "dashboard":
      if (appState.view === "dashboard-monitor") return "dashboard-monitor";
      if (appState.view === "dashboard-todos") return "dashboard-todos";
      if (appState.view === "dashboard-warnings") return "dashboard-warnings";
      return "dashboard-home";
    case "resources":
      return appState.resourceGroup;
    case "systems":
      return "systems-directory";
    case "assets":
      return appState.assetModule;
    case "query":
      return appState.queryModule;
    case "services":
      if (appState.view === "access-detail" || (appState.view === "services" && appState.shareModule === "access")) return "access";
      return "share";
    case "analysis":
      return appState.analysisModule;
    case "permissions":
      return appState.permissionModule;
    case "governance":
      return "governance-overview";
    default:
      return "";
  }
});

const currentUser = computed(() => userDirectory.find((item) => item.id === appState.currentUserId) ?? userDirectory[0]);
const currentUserInitial = computed(() => currentUser.value?.name?.slice(0, 1) ?? "管");
const currentUserAreaLabel = computed(() => {
  const user = currentUser.value;
  if (!user) return "贵州省";
  if (user.scope === "全省") return "贵州省";
  if (user.scope === "按条线") return user.org;
  if (user.scope === "本单位") return user.org;
  return user.scope;
});
const portalDate = new Date();
const portalDateText = computed(() => `${portalDate.getFullYear()}年${portalDate.getMonth() + 1}月${portalDate.getDate()}日`);
const welcomeGreeting = computed(() => {
  const hour = portalDate.getHours();
  if (hour < 12) return "上午好";
  if (hour < 18) return "下午好";
  return "晚上好";
});
const onlineUserCount = computed(() => userDirectory.filter((item) => item.loginState === "在线").length);
const enabledUserCount = computed(() => userDirectory.filter((item) => item.status === "启用").length);
const activeSessionCount = computed(() =>
  userDirectory.filter((item) => item.currentSession === "当前会话" || item.currentSession === "活跃会话").length,
);
const loginAccountOptions = computed(() => userDirectory.filter((item) => item.status === "启用"));
const pageBreadcrumb = computed(() => {
  const primaryLabel = navigation.find((item) => item.id === currentPrimaryNavId.value)?.label ?? "首页";
  if (currentPrimaryNavId.value === "dashboard") return `首页 / ${selectedNav.value.label}`;
  if (selectedNav.value.label === primaryLabel) return `首页 / ${primaryLabel}`;
  return `${primaryLabel} / ${selectedNav.value.label}`;
});
const permissionOverviewCards = computed(() => [
  { title: "系统用户", value: `${userDirectory.length} 人`, meta: `启用 ${enabledUserCount.value} 人` },
  { title: "在线用户", value: `${onlineUserCount.value} 人`, meta: `当前活跃会话 ${activeSessionCount.value} 个` },
  { title: "角色模板", value: `${roleDirectory.length} 个`, meta: "覆盖省级、条线、辖区、本单位" },
  { title: "组织机构", value: `${orgDirectory.length} 个`, meta: "支持分级分域授权" },
]);
const shareTertiaryLabel = computed(() => shareTertiaryItems.find((item) => item.id === activeShareTertiaryId.value)?.label ?? "共享申请");

const queryCityOptions = [...new Set(resources.map((item) => item.city))];
const queryCountyOptions = [...new Set(resources.map((item) => item.county))];
const queryScenicOptions = resources.filter((item) => item.category === "景区").map((item) => item.name).slice(0, 12);
const queryAreaLevelOptions = ["省", "市州", "区县"];
const queryCityResourceCountMap = Object.fromEntries(
  queryCityOptions.map((city) => [city, resources.filter((item) => item.city === city).length]),
);
const queryScenicWeightMap = Object.fromEntries(
  queryScenicOptions.map((name, index) => [name, 0.18 - index * 0.008]),
);
const queryCityWeightTotal = queryCityOptions.reduce((sum, city) => sum + queryCityResourceCountMap[city] + 4, 0);
const queryCityWeightMap = Object.fromEntries(
  queryCityOptions.map((city) => [city, (queryCityResourceCountMap[city] + 4) / queryCityWeightTotal]),
);
const queryLevelOptions = ["全部", ...new Set(resources.map((item) => item.level))];
const queryChargeOptions = ["全部", ...new Set(resources.map((item) => item.chargeMode))];
const queryResourceCategoryOptions = ["全部", ...allResourceCategories];
const queryGeoDimensions = ["市州", "区县", "景区"];
const queryResourceBaseDimensions = ["市州", "区县"];
const queryFoundationDimensions = ["月份", "数据类型", "来源渠道", "共享类型", "责任单位", "应用场景"];
const queryAreaModuleDefaults = {
  theme: "area-query",
  metricId: "area_flow",
  timePreset: "month-2026-07",
  granularity: "日",
  region: "全省",
  dimension: "市州",
  source: "综合口径",
  resourceCategory: "全部",
  levelFilter: "全部",
  chargeFilter: "全部",
};
const queryScenicModuleDefaults = {
  theme: "scenic-query",
  metricId: "scenic_reception",
  timePreset: "month-2026-07",
  granularity: "日",
  region: queryScenicOptions[0] ?? "黄果树旅游区",
  dimension: "景区",
  source: "综合口径",
  resourceCategory: "全部",
  levelFilter: "全部",
  chargeFilter: "全部",
};
const queryDedicatedModuleMeta = {
  "query-area": {
    eyebrow: "区域数据查询",
    title: "区域客流与游客画像查询",
    description: "支持区域客流量、区域游客画像对接数据查询、导出，并按日、周、月、季度、半年、年、节假日维度统计。",
    theme: "area-query",
    defaultConfig: queryAreaModuleDefaults,
    scopeLabel: "区域范围",
    scopeOptions: ["全省", ...queryCityOptions, ...queryCountyOptions],
    quickDimensions: ["市州", "区县", "性别", "年龄", "游客来源地", "出行方式", "夜游情况"],
  },
  "query-scenic": {
    eyebrow: "景区数据查询",
    title: "游客接待、画像与预约查询",
    description: "支持游客接待、游客画像和游客预约数据查询、导出，覆盖小时、日、周、月、季度、半年、年及节假日统计。",
    theme: "scenic-query",
    defaultConfig: queryScenicModuleDefaults,
    scopeLabel: "景区名称",
    scopeOptions: queryScenicOptions,
    quickDimensions: ["景区", "接待类型", "性别", "年龄", "游客停留时长", "游客来源地", "预约渠道"],
  },
};
const queryActionMessage = ref("选择关键指标后，可按时间、区域和主题维度开展自助查询。");
const savedQueryItems = reactive([
  {
    id: "saved-1",
    name: "全省景区资源底账",
    theme: "resource",
    metricId: "scenic_count",
    timePreset: "year-2026",
    granularity: "当前",
    region: "全省",
    dimension: "市州",
    source: "旅游资源主数据",
    updatedAt: "2026-07-11 09:10",
    summary: "全省景区数量，按市州查看分布并支持下钻明细。",
  },
  {
    id: "saved-2",
    name: "7月每日客流监测",
    theme: "flow",
    metricId: "lbs_flow",
    timePreset: "month-2026-07",
    granularity: "日",
    region: "全省",
    dimension: "市州",
    source: "综合口径",
    updatedAt: "2026-07-11 08:45",
    summary: "查看 7 月每日 LBS 客流变化和分市州分布。",
  },
  {
    id: "saved-3",
    name: "视频在线率日监测",
    theme: "emergency",
    metricId: "video_online_rate",
    timePreset: "recent-7",
    granularity: "日",
    region: "全省",
    dimension: "市州",
    source: "监控平台",
    updatedAt: "2026-07-10 18:20",
    summary: "查看近 7 日视频在线率变化及责任单位分布。",
  },
]);

const activeQueryTheme = computed(() => queryThemeCatalog.find((item) => item.id === appState.queryTheme) ?? queryThemeCatalog[0]);
const queryMetricOptions = computed(() => queryMetricCatalog.filter((item) => item.theme === appState.queryTheme));
const activeQueryMetric = computed(() => queryMetricCatalog.find((item) => item.id === appState.queryMetricId) ?? queryMetricOptions.value[0] ?? queryMetricCatalog[0]);
const executedQueryTheme = computed(() => queryThemeCatalog.find((item) => item.id === appState.queryExecuted.theme) ?? activeQueryTheme.value);
const executedQueryMetric = computed(() => queryMetricCatalog.find((item) => item.id === appState.queryExecuted.metricId) ?? activeQueryMetric.value);
const isDraftResourceQuery = computed(() => activeQueryMetric.value?.mode === "resource");
const showQueryTimeFields = computed(() => !isDraftResourceQuery.value);
const showQueryRegionField = computed(() => activeQueryMetric.value?.theme !== "foundation");
const showQueryResourceFields = computed(() => activeQueryMetric.value?.theme === "resource");
const showQuerySourceField = computed(() => (activeQueryMetric.value?.sources?.length ?? 0) > 1);
const activeDedicatedQueryMeta = computed(() => queryDedicatedModuleMeta[appState.queryModule]);
const currentQueryModuleLabel = computed(
  () => queryModules.find((item) => item.id === appState.queryModule)?.label ?? activeDedicatedQueryMeta.value?.eyebrow ?? "查询中心",
);
const dedicatedQueryMetricOptions = computed(() =>
  activeDedicatedQueryMeta.value
    ? queryMetricCatalog.filter((item) => item.theme === activeDedicatedQueryMeta.value.theme)
    : [],
);
const dedicatedQueryDimensionOptions = computed(() => {
  if (!activeDedicatedQueryMeta.value) return [];
  const metric = activeQueryMetric.value;
  const baseDimensions = metric?.theme === "area-query"
    ? ["市州", "区县"]
    : ["景区"];
  return [...new Set([...baseDimensions, ...(metric?.dimensions ?? []), ...activeDedicatedQueryMeta.value.quickDimensions])];
});
const queryGranularityOptions = computed(() => {
  const metric = activeQueryMetric.value;
  if (!metric || metric.mode === "resource") return ["当前"];
  const metricOptions = new Set([...(metric.granularityOptions ?? ["日", "月"]), "年"]);
  if (appState.queryTimePreset === "holiday-2026") {
    return metricOptions.has("节假日") ? ["节假日"] : [...metricOptions];
  }
  const periodOptions = appState.queryTimePreset === "year-2026"
    ? ["月", "季度", "半年", "年"]
    : appState.queryTimePreset === "month-2026-07"
      ? ["小时", "日", "周", "月"]
      : ["日", "周"];
  const options = periodOptions.filter((item) => metricOptions.has(item));
  return options.length ? options : [...metricOptions];
});
const queryDimensionOptions = computed(() => {
  const metric = activeQueryMetric.value;
  if (!metric) return ["市州"];
  if (metric.theme === "area-query") {
    return ["市州", "区县", "区域层级", ...(metric.dimensions ?? []).filter((item) => item !== "市州" && item !== "区县" && item !== "区域层级")];
  }
  if (metric.theme === "scenic-query") {
    return ["景区", ...(metric.dimensions ?? []).filter((item) => item !== "景区")];
  }
  if (metric.theme === "resource") {
    return [...new Set([...queryResourceBaseDimensions, ...(metric.dimensions ?? [])])];
  }
  if (metric.theme === "foundation") {
    return [...new Set([...(metric.dimensions ?? []), ...queryFoundationDimensions])];
  }
  return [...new Set([...queryGeoDimensions, ...(metric.dimensions ?? []).filter((item) => item !== "市州")])];
});
const querySourceOptions = computed(() => ["全部", ...(activeQueryMetric.value?.sources ?? ["综合口径"])]);
const queryOverviewCards = computed(() => [
  { title: "核心指标", value: `${queryMetricCatalog.length} 项`, meta: "覆盖客流、资源、消费、应急、底座等主题" },
  { title: "主题域", value: `${queryThemeCatalog.length} 类`, meta: "支持按主题和按指标双入口查询" },
  { title: "专题报表", value: `${queryReportTemplates.length} 套`, meta: "可直接套用处室常用专题模板" },
  { title: "我的查询", value: `${savedQueryItems.length} 项`, meta: "支持保存、复用和导出当前查询结果" },
]);
const queryThemeCards = computed(() =>
  queryThemeCatalog.map((item) => ({
    ...item,
    metricCount: item.metrics.length,
    hotMetrics: item.metrics
      .map((metricId) => queryMetricCatalog.find((metric) => metric.id === metricId)?.label)
      .filter(Boolean)
      .slice(0, 3),
  })),
);
const queryOverviewHotMetrics = computed(() => queryMetricCatalog.slice(0, 8));
const querySavedCurrentItem = computed(() => savedQueryItems.find((item) => item.metricId === appState.queryExecuted.metricId && item.region === appState.queryExecuted.region));
const currentQueryResult = computed(() => buildQueryResult(executedQueryMetric.value, appState.queryExecuted));
const currentQuerySummaryCards = computed(() => currentQueryResult.value.summaryCards ?? []);
const currentQueryTrendPoints = computed(() => currentQueryResult.value.trendPoints ?? []);
const currentQueryTrendChart = computed(() => {
  const points = currentQueryTrendPoints.value;
  const width = 1000;
  const height = 320;
  const paddingX = 42;
  const paddingTop = 34;
  const paddingBottom = 60;
  const innerWidth = width - paddingX * 2;
  const innerHeight = height - paddingTop - paddingBottom;
  const maxValue = Math.max(...points.map((point) => point.value), 1);
  const stepX = points.length > 1 ? innerWidth / (points.length - 1) : 0;
  const labelStep = points.length > 18 ? 3 : points.length > 12 ? 2 : 1;
  const chartPoints = points.map((point, index) => {
    const x = paddingX + stepX * index;
    const y = paddingTop + (1 - point.value / maxValue) * innerHeight;
    return {
      ...point,
      x,
      y,
      showLabel: index % labelStep === 0 || index === points.length - 1,
    };
  });
  const linePath = chartPoints.map((point, index) => `${index === 0 ? "M" : "L"} ${point.x.toFixed(1)} ${point.y.toFixed(1)}`).join(" ");
  const areaPath = chartPoints.length
    ? `${linePath} L ${chartPoints.at(-1).x.toFixed(1)} ${height - paddingBottom} L ${chartPoints[0].x.toFixed(1)} ${height - paddingBottom} Z`
    : "";
  return {
    width,
    height,
    paddingBottom,
    chartBottom: height - paddingBottom,
    chartPoints,
    linePath,
    areaPath,
    gridLines: [0, 25, 50, 75, 100].map((ratio) => ({
      ratio,
      y: paddingTop + (ratio / 100) * innerHeight,
    })),
  };
});
const currentQueryDistributionRows = computed(() => currentQueryResult.value.distributionRows ?? []);
const currentQueryMatrix = computed(() => currentQueryResult.value.matrix ?? { columns: [], rows: [], rowHeader: "维度", columnHeader: "统计项" });
const currentQueryMatrixCaption = computed(() => {
  const matrix = currentQueryMatrix.value;
  return `${matrix.rowHeader ?? "维度"} × ${matrix.columnHeader ?? "统计项"}`;
});
const currentQueryTable = computed(() => currentQueryResult.value.table ?? { columns: [], rows: [] });
const currentQueryMetricAssets = computed(() => findAssetsForQueryMetric(executedQueryMetric.value).slice(0, 4));
const currentQueryRelatedMetric = computed(() =>
  metrics.find((item) => item.name === executedQueryMetric.value?.relatedMetricName || item.name === executedQueryMetric.value?.label),
);
const isResourceQuery = computed(() => executedQueryMetric.value?.mode === "resource");
const currentQueryTimeLabel = computed(
  () => queryTimePresetOptions.find((item) => item.id === appState.queryExecuted.timePreset)?.label ?? appState.queryExecuted.timePreset,
);
const currentQueryResultViewLabel = computed(
  () => queryResultViews.find((item) => item.id === appState.queryResultView)?.label ?? "趋势分析",
);
const currentQueryColumns = computed(() => currentQueryTable.value.columns ?? []);
const currentQueryRows = computed(() => currentQueryTable.value.rows ?? []);

function secondaryNavCount(id) {
  if (currentPrimaryNavId.value === "analysis") return "";
  if (currentPrimaryNavId.value === "dashboard") {
    if (id === "dashboard-home") return "";
    if (id === "dashboard-monitor") return "监控";
    if (id === "dashboard-todos") return `${dashboardTodoItems.length}`;
    if (id === "dashboard-warnings") return `${dashboardWarningItems.length}`;
  }
  if (currentPrimaryNavId.value === "permissions") {
    if (id === "users") return `${userDirectory.length}`;
    if (id === "roles") return `${roleDirectory.length}`;
    if (id === "orgs") return `${orgDirectory.length}`;
    return "";
  }

  if (currentPrimaryNavId.value === "services") {
    if (id === "access") return `${filteredAccessItems.value.length}`;
    if (id === "share") return `${shareApplications.value.length}`;
    return "";
  }

  if (currentPrimaryNavId.value === "assets") {
    if (id === "asset-overview") return "";
    if (id === "asset-indicators") return `${assetIndicatorTotal.value}`;
    if (assetModuleTypeMap[id]) {
      return `${dataAssets.filter((item) => assetModuleTypeMap[id] === "全部" || item.type === assetModuleTypeMap[id]).length}`;
    }
    return "";
  }

  if (currentPrimaryNavId.value === "query") {
    if (id === "query-overview") return "";
    if (id === "query-explore") return `${queryMetricCatalog.length}`;
    if (id === "query-reports") return `${queryReportTemplates.length}`;
    if (id === "query-favorites") return `${savedQueryItems.length}`;
    return "";
  }

  if (currentPrimaryNavId.value === "resources") {
    const targetGroup = resourceGroups.find((item) => item.id === id);
    if (targetGroup) {
      return `${resources.filter((item) => targetGroup.categories.includes(item.category)).length}`;
    }
  }
  if (id === "systems-directory") return `${filteredSystems.value.length}`;
  return "";
}

function stringValue(value) {
  if (Array.isArray(value)) return value.join("、");
  return String(value ?? "");
}

function keywordMatch(keyword, values) {
  return !keyword || values.join(" ").includes(keyword);
}

function resourceSearchText(item) {
  const detailValues = (item.detailSections ?? [])
    .flatMap((group) => group.fields)
    .flatMap((entry) => (Array.isArray(entry.value) ? entry.value : [String(entry.value ?? "")]));

  return [
    item.name,
    ...(item.aliases ?? []),
    item.category,
    item.level,
    item.city,
    item.county,
    item.address,
    item.intro,
    ...item.linkedData,
    ...detailValues,
  ].join(" ");
}

function findResourceField(item, labels) {
  for (const group of item.detailSections ?? []) {
    for (const entry of group.fields ?? []) {
      if (labels.includes(entry.label)) return stringValue(entry.value);
    }
  }
  return "";
}

const resourceLevelColumnLabel = computed(() => (appState.resourceCategory === "景区" ? "景区等级" : "资源等级"));

const resourceHiddenFieldLabels = {
  街区: ["营业状态", "是否收费", "收费情况"],
  乡村旅游点: ["营业状态"],
  酒店民宿: ["收费情况", "是否收费", "开放时间"],
  旅行社: ["营业状态", "经营状态", "收费情况", "是否收费", "开放时间"],
  导游: ["营业状态", "执业状态", "收费情况", "是否收费", "开放时间"],
  餐饮主体: ["收费情况", "是否收费", "开放时间"],
  购物主体: ["收费情况", "是否收费", "开放时间"],
  旅游服务主体: ["收费情况", "是否收费", "开放时间"],
};

function filterResourceSections(item) {
  const hiddenLabels = new Set(resourceHiddenFieldLabels[item.category] ?? []);
  if (!hiddenLabels.size) return item.detailSections ?? [];
  return (item.detailSections ?? [])
    .map((group) => ({
      ...group,
      fields: (group.fields ?? []).filter((entry) => !hiddenLabels.has(entry.label)),
    }))
    .filter((group) => group.fields.length > 0);
}

const showResourceStatusColumn = computed(() => !["街区", "乡村旅游点", "旅行社", "导游", "餐饮主体", "购物主体", "旅游服务主体"].includes(appState.resourceCategory));
const showResourceChargeColumn = computed(() => !["街区", "酒店民宿", "旅行社", "导游", "餐饮主体", "购物主体", "旅游服务主体"].includes(appState.resourceCategory));
const showResourceOpeningColumn = computed(() => !["酒店民宿", "旅行社", "导游", "餐饮主体", "购物主体", "旅游服务主体"].includes(appState.resourceCategory));

function resourceOperationStatus(item) {
  return findResourceField(item, ["营业状态", "当前经营状态"]) || "正常开放";
}

function isFullSpanField(entry) {
  if (entry.full || Array.isArray(entry.value)) return true;
  return String(entry.value ?? "").length > 24;
}

const filteredResources = computed(() => {
  const keyword = appState.query.trim();
  return resources.filter((item) => {
    const categoryMatch = item.category === appState.resourceCategory;
    return categoryMatch && keywordMatch(keyword, [resourceSearchText(item)]);
  });
});

const selectedResource = computed(() => resources.find((item) => item.id === appState.selectedResourceId) ?? resources[0]);

const selectedResourceSections = computed(() => (selectedResource.value ? filterResourceSections(selectedResource.value) : []));
const selectedResourceFieldCount = computed(() => selectedResourceSections.value.reduce((count, group) => count + group.fields.length, 0));

const resourceStats = computed(() => {
  return [
    { label: "资源主体类型", value: `${resourceCategories.value.length} 类` },
    { label: "当前目录条数", value: `${filteredResources.value.length} 条` },
    { label: "已关联数据项", value: `${selectedResource.value?.linkedData.length ?? 0} 项` },
  ];
});

const filteredSystems = computed(() => {
  const keyword = appState.query.trim();
  return businessSystems.filter((item) => {
    return item.type === "业务系统" && keywordMatch(keyword, [item.name, item.ownerDept, item.content, item.purpose, item.status]);
  });
});

const selectedSystem = computed(() => {
  return filteredSystems.value.find((item) => item.id === appState.selectedSystemId) ?? filteredSystems.value[0] ?? businessSystems.find((item) => item.type === "业务系统");
});

const filteredAssets = computed(() => {
  const keyword = appState.query.trim();
  return dataAssets.filter((item) => {
    const typeMatch = appState.assetType === "全部" || item.type === appState.assetType;
    return typeMatch && keywordMatch(keyword, [item.name, item.type, item.source, item.serviceScenes, item.status]);
  });
});

const isAssetDirectoryModule = computed(() => assetDirectoryModuleIds.includes(appState.assetModule));
const currentAssetDirectoryTitle = computed(() => {
  return assetNavigationItems.find((item) => item.id === appState.assetModule)?.label ?? "主数据资产";
});
const currentAssetDirectoryEyebrow = computed(() => {
  if (appState.assetType === "全部") return "资产清单";
  return "资产类型";
});

const selectedAsset = computed(() => {
  if (appState.view === "asset-detail") {
    return dataAssets.find((item) => item.id === appState.selectedAssetId) ?? filteredAssets.value[0] ?? dataAssets[0];
  }
  return filteredAssets.value.find((item) => item.id === appState.selectedAssetId) ?? filteredAssets.value[0] ?? dataAssets[0];
});

const metricDomainOptions = computed(() => ["全部", ...new Set(metrics.map((item) => item.domain))]);

const tableMetrics = computed(() => {
  const keyword = appState.metricQuery.trim();
  return metrics.filter((item) => {
    const domainMatch = appState.metricDomain === "全部" || item.domain === appState.metricDomain;
    return domainMatch && keywordMatch(keyword, [item.name]);
  });
});

const selectedMetric = computed(() => {
  if (appState.view === "metric-detail") {
    return metrics.find((item) => item.id === appState.selectedMetricId) ?? tableMetrics.value[0] ?? metrics[0];
  }
  return tableMetrics.value.find((item) => item.id === appState.selectedMetricId) ?? tableMetrics.value[0] ?? metrics[0];
});

const filteredServices = computed(() => {
  const keyword = appState.query.trim();
  return serviceCatalog.filter((item) => {
    const typeMatch = appState.serviceType === "全部" || item.type === appState.serviceType;
    return typeMatch && keywordMatch(keyword, [item.name, item.type, item.provider, item.target, item.detail, item.status]);
  });
});

const selectedService = computed(() => {
  if (appState.view === "service-detail") {
    return serviceCatalog.find((item) => item.id === appState.selectedServiceId) ?? filteredServices.value[0] ?? serviceCatalog[0];
  }
  return filteredServices.value.find((item) => item.id === appState.selectedServiceId) ?? filteredServices.value[0] ?? serviceCatalog[0];
});

const filteredShareApplications = computed(() => {
  const keyword = appState.shareKeyword.trim();
  return shareApplications.value.filter((item) => {
    const typeMatch = appState.shareTypeFilter === "全部" || item.shareType === appState.shareTypeFilter;
    const statusMatch = appState.shareStatusFilter === "全部" || item.status === appState.shareStatusFilter;
    return (
      typeMatch &&
      statusMatch &&
      keywordMatch(keyword, [item.applicationNo, item.serviceName, item.shareType, item.applyUnit, item.applicant, item.status, item.currentStep])
    );
  });
});

const selectedShareApplication = computed(() => {
  return shareApplications.value.find((item) => item.id === appState.selectedShareApplicationId) ?? shareApplications.value[0];
});

const selectedShareSourceService = computed(() => {
  return serviceCatalog.find((item) => item.id === selectedShareApplication.value?.serviceId) ?? selectedService.value ?? serviceCatalog[0];
});

const accessTypeOptions = computed(() => ["全部", ...new Set(dataAccessCatalog.map((item) => item.dataType))]);
const accessStatusOptions = computed(() => ["全部", ...new Set(dataAccessCatalog.map((item) => item.status))]);
const shareReviewStepOptions = computed(() => ["全部", ...new Set(shareApplications.value.map((item) => item.currentStep))]);
const sharedDataMethodOptions = computed(() => ["全部", ...new Set(sharedDataCatalog.value.map((item) => item.shareMethod))]);
const sharedDataStatusOptions = computed(() => ["全部", ...new Set(sharedDataCatalog.value.map((item) => item.status))]);
const sharedCallMethodOptions = computed(() => ["全部", ...new Set(sharedDataCallCatalog.value.map((item) => item.callMethod))]);
const sharedCallStatusOptions = computed(() => ["全部", ...new Set(sharedDataCallCatalog.value.map((item) => item.callStatus))]);
const servicesPageSizeOptions = [10, 20, 50];

const filteredSharedDataItems = computed(() => {
  const keyword = appState.sharedDataKeyword.trim();
  return sharedDataCatalog.value.filter((item) => {
    const methodMatch = appState.sharedDataMethodFilter === "全部" || item.shareMethod === appState.sharedDataMethodFilter;
    const statusMatch = appState.sharedDataStatusFilter === "全部" || item.status === appState.sharedDataStatusFilter;
    return (
      methodMatch &&
      statusMatch &&
      keywordMatch(keyword, [item.dataName, item.tableName, item.shareMethod, item.requestNo, item.serviceName, item.status])
    );
  });
});

const selectedSharedData = computed(() => {
  return sharedDataCatalog.value.find((item) => item.id === appState.selectedSharedDataId) ?? sharedDataCatalog.value[0];
});

const selectedSharedDataRequest = computed(() => {
  return shareApplications.value.find((item) => item.id === selectedSharedData.value?.requestId) ?? selectedShareApplication.value;
});

const filteredSharedCallItems = computed(() => {
  const keyword = appState.sharedCallKeyword.trim();
  return sharedDataCallCatalog.value.filter((item) => {
    const methodMatch = appState.sharedCallMethodFilter === "全部" || item.callMethod === appState.sharedCallMethodFilter;
    const statusMatch = appState.sharedCallStatusFilter === "全部" || item.callStatus === appState.sharedCallStatusFilter;
    return (
      methodMatch &&
      statusMatch &&
      keywordMatch(keyword, [item.dataName, item.requestNo, item.serviceName, item.callerUnit, item.callMethod, item.callStatus])
    );
  });
});

const filteredAccessItems = computed(() => {
  const keyword = appState.accessKeyword.trim();
  return dataAccessCatalog.filter((item) => {
    const typeMatch = appState.accessTypeFilter === "全部" || item.dataType === appState.accessTypeFilter;
    const statusMatch = appState.accessStatusFilter === "全部" || item.status === appState.accessStatusFilter;
    return (
      typeMatch &&
      statusMatch &&
      keywordMatch(keyword, [item.name, item.dataType, item.sourceChannel, item.accessDate, item.refresh, item.status, item.originSystem])
    );
  });
});

const filteredShareReviewApplications = computed(() => {
  const keyword = appState.shareReviewKeyword.trim();
  return shareApplications.value.filter((item) => {
    const statusMatch = appState.shareReviewStatusFilter === "全部" || item.status === appState.shareReviewStatusFilter;
    const stepMatch = appState.shareReviewStepFilter === "全部" || item.currentStep === appState.shareReviewStepFilter;
    return (
      statusMatch &&
      stepMatch &&
      keywordMatch(keyword, [item.applicationNo, item.serviceName, item.applyUnit, item.currentStep, item.currentReviewer, item.status])
    );
  });
});

function paginateItems(items, page, pageSize) {
  const start = (page - 1) * pageSize;
  return items.slice(start, start + pageSize);
}

const pagedAccessItems = computed(() => paginateItems(filteredAccessItems.value, appState.accessPage, appState.servicesPageSize));
const pagedShareApplications = computed(() => paginateItems(filteredShareApplications.value, appState.shareListPage, appState.servicesPageSize));
const pagedShareReviewApplications = computed(() =>
  paginateItems(filteredShareReviewApplications.value, appState.shareReviewPage, appState.servicesPageSize),
);
const pagedSharedDataItems = computed(() => paginateItems(filteredSharedDataItems.value, appState.sharedDataPage, appState.servicesPageSize));
const pagedSharedCallItems = computed(() => paginateItems(filteredSharedCallItems.value, appState.sharedCallPage, appState.servicesPageSize));

const selectedAccessItem = computed(() => {
  if (appState.view === "access-detail") {
    return dataAccessCatalog.find((item) => item.id === appState.selectedAccessId) ?? filteredAccessItems.value[0] ?? dataAccessCatalog[0];
  }
  return filteredAccessItems.value.find((item) => item.id === appState.selectedAccessId) ?? filteredAccessItems.value[0] ?? dataAccessCatalog[0];
});

const selectedUser = computed(() => userDirectory.find((item) => item.id === appState.selectedUserId) ?? userDirectory[0]);
const selectedRole = computed(() => roleDirectory.find((item) => item.id === appState.selectedRoleId) ?? roleDirectory[0]);
const selectedOrg = computed(() => orgDirectory.find((item) => item.id === appState.selectedOrgId) ?? orgDirectory[0]);

const resourceCategorySummary = computed(() => {
  return allResourceCategories
    .map((category) => ({
      category,
      count: resources.filter((item) => item.category === category).length,
    }));
});

const systemSummary = computed(() => {
  const systemList = businessSystems.filter((item) => item.type === "业务系统");
  return [
    { title: "业务系统总数", value: `${systemList.length} 项` },
    { title: "已配置入口", value: `${systemList.filter((item) => item.loginUrl).length} 项` },
    { title: "暂不可跳转", value: `${systemList.filter((item) => !item.loginUrl).length} 项` },
  ];
});

const visibleDataAssets = computed(() => dataAssets.filter((item) => item.type !== "主题数据资产"));

const assetSummary = computed(() => {
  return assetTypes
    .filter((item) => item !== "全部" && item !== "指标资产" && item !== "主题数据资产")
    .map((type) => ({ title: type, value: `${visibleDataAssets.value.filter((item) => item.type === type).length} 项` }));
});

const assetIndicatorTotal = computed(() => dataAssets.filter((item) => item.type === "指标资产").length + metrics.length);
const assetCenterTotal = computed(() => visibleDataAssets.value.length + metrics.length);

const assetTypeOverviewRows = computed(() => {
  const descriptionMap = {
    主数据资产: "资源、住宿、文化、涉旅主体等统一底账",
    标签资产: "游客画像、主体画像和风险标签",
    知识资产: "智能体问答、知识库和政策规则",
    服务资产: "接口、共享服务和智能服务能力",
    指标资产: "统一口径、维度粒度和应用场景",
  };
  const rows = assetTypes
    .filter((item) => item !== "全部" && item !== "指标资产" && item !== "主题数据资产")
    .map((type) => ({
      title: type,
      count: visibleDataAssets.value.filter((asset) => asset.type === type).length,
      description: descriptionMap[type] ?? "可进入资产目录查看详情",
    }));
  rows.push({
    title: "指标资产",
    count: assetIndicatorTotal.value,
    description: descriptionMap["指标资产"],
  });
  return rows
    .map((item) => ({
      ...item,
      ratio: assetCenterTotal.value ? Math.round((item.count / assetCenterTotal.value) * 100) : 0,
    }))
    .sort((a, b) => b.count - a.count);
});

const assetCenterOverviewCards = computed(() => [
  { title: "资产总量", value: `${assetCenterTotal.value} 项`, meta: "含数据资产与指标资产" },
  { title: "数据资产", value: `${visibleDataAssets.value.length} 项`, meta: "主数据、标签、知识与服务" },
  { title: "指标资产", value: `${assetIndicatorTotal.value} 项`, meta: "统一口径、统一来源、统一复用" },
  { title: "可共享资产", value: `${visibleDataAssets.value.filter((item) => item.shareStatus !== "内部使用").length + metrics.length} 项`, meta: "支撑共享申请和授权使用" },
]);

const dashboardAssetCatalogEntries = computed(() => [
  ...dataAssets,
  ...metrics.map((item) => ({
    ...item,
    type: "指标资产",
    status: "已发布",
    ownerDept: item.owner,
    shareStatus: "已发布",
    serviceMode: "指标服务 + 智能问数",
  })),
]);

const dashboardAssetTypeRows = computed(() => {
  const typeOrder = ["主数据资产", "主题数据资产", "标签资产", "知识资产", "指标资产", "服务资产"];
  const descriptionMap = {
    主数据资产: "统一资源、住宿、文化和涉旅主体底账",
    主题数据资产: "围绕客流、住宿、监管、信用和运行专题沉淀",
    标签资产: "支撑客群画像、消费热度和信用分层",
    知识资产: "服务智能体问答、知识检索和视频专题",
    指标资产: "统一指标口径、来源、刷新频率和应用场景",
    服务资产: "面向共享交换、智能应用和业务系统调用",
  };
  return typeOrder.map((type) => {
    const entries = dashboardAssetCatalogEntries.value.filter((item) => item.type === type);
    const readyCount = entries.filter((item) => item.status?.includes("已")).length;
    const buildingCount = entries.filter((item) => item.status?.includes("建设")).length;
    return {
      type,
      count: entries.length,
      readyCount,
      buildingCount,
      ratio: dashboardAssetCatalogEntries.value.length ? Math.round((entries.length / dashboardAssetCatalogEntries.value.length) * 100) : 0,
      description: descriptionMap[type],
      owner: [...new Set(entries.map((item) => item.ownerDept).filter(Boolean))].slice(0, 2).join("、") || "数据办",
      refresh: [...new Set(entries.map((item) => item.refresh).filter(Boolean))].slice(0, 2).join(" / ") || "按需更新",
      shareStatus: [...new Set(entries.map((item) => item.shareStatus).filter(Boolean))].slice(0, 2).join("、") || "按目录授权",
    };
  });
});

const dashboardAssetMonitorCards = computed(() => {
  const entries = dashboardAssetCatalogEntries.value;
  const readyCount = entries.filter((item) => item.status?.includes("已")).length;
  const buildingCount = entries.filter((item) => item.status?.includes("建设")).length;
  const sharedCount = entries.filter((item) => item.shareStatus && item.shareStatus !== "内部使用").length;
  return [
    { label: "目录资产总量", value: `${entries.length}项`, meta: "数据资产目录 + 指标目录" },
    { label: "资产分类", value: `${dashboardAssetTypeRows.value.filter((item) => item.count > 0).length}类`, meta: "主数据、主题、标签、知识、指标、服务" },
    { label: "已沉淀/发布", value: `${readyCount}项`, meta: "可纳入应用和共享服务" },
    { label: "建设中资产", value: `${buildingCount}项`, meta: "持续接入治理与完善" },
    { label: "可共享资产", value: `${sharedCount}项`, meta: "按授权、接口或目录方式使用" },
  ];
});

const assetThemeDomainRows = computed(() =>
  metricDomainOptions.value
    .filter((item) => item !== "全部")
    .map((domain) => ({
      domain,
      metricCount: metrics.filter((item) => item.domain === domain).length,
      assetCount: dataAssets.filter((item) => item.name.includes(domain.slice(0, 2)) || item.serviceScenes.includes(domain)).length,
      owner: metrics.find((item) => item.domain === domain)?.owner ?? "数据办",
      scenes: [...new Set(metrics.filter((item) => item.domain === domain).map((item) => item.scenes).filter(Boolean))].slice(0, 2).join("；") || "驾驶舱、智能问数、共享服务",
    })),
);

const serviceSummary = computed(() => {
  return serviceTypes
    .filter((item) => item !== "全部")
    .map((type) => ({ title: type, value: `${serviceCatalog.filter((item) => item.type === type).length} 项` }));
});

const accessSummary = computed(() => {
  return [
    { title: "数据接入总量", value: `${dataAccessCatalog.length} 项` },
    { title: "已接入", value: `${dataAccessCatalog.filter((item) => item.status === "已接入").length} 项` },
    { title: "建设中", value: `${dataAccessCatalog.filter((item) => item.status !== "已接入").length} 项` },
  ];
});

const currentServicesStatusSummary = computed(() => {
  if (appState.shareModule === "access") {
    return accessSummary.value;
  }

  if (appState.shareTertiary === "share-review") {
    return [
      { title: "待责任单位确认", value: `${shareApplications.value.filter((item) => item.status === "待责任单位确认").length} 项` },
      { title: "审核中", value: `${shareApplications.value.filter((item) => item.status === "审核中").length} 项` },
      { title: "补充材料", value: `${shareApplications.value.filter((item) => item.status === "补充材料").length} 项` },
      { title: "已办结", value: `${shareApplications.value.filter((item) => item.status === "已开通").length} 项` },
    ];
  }

  if (appState.shareTertiary === "shared-data") {
    return [
      { title: "共享清单总量", value: `${sharedDataCatalog.value.length} 项` },
      { title: "表下载", value: `${sharedDataCatalog.value.filter((item) => item.shareMethod === "表下载").length} 项` },
      { title: "接口推送", value: `${sharedDataCatalog.value.filter((item) => item.shareMethod === "接口推送").length} 项` },
      { title: "已开通", value: `${sharedDataCatalog.value.filter((item) => item.status === "已开通").length} 项` },
    ];
  }

  if (appState.shareTertiary === "share-calls") {
    return [
      { title: "调用记录总量", value: `${sharedDataCallCatalog.value.length} 条` },
      { title: "调用成功", value: `${sharedDataCallCatalog.value.filter((item) => item.callStatus === "成功").length} 条` },
      { title: "调用失败", value: `${sharedDataCallCatalog.value.filter((item) => item.callStatus === "失败").length} 条` },
      { title: "共享数据量", value: "28.8万行" },
    ];
  }

  return [
    { title: "共享申请总量", value: `${shareApplications.value.length} 项` },
    { title: "待责任单位确认", value: `${shareApplications.value.filter((item) => item.status === "待责任单位确认").length} 项` },
    { title: "审核中", value: `${shareApplications.value.filter((item) => item.status === "审核中").length} 项` },
    { title: "已开通", value: `${shareApplications.value.filter((item) => item.status === "已开通").length} 项` },
  ];
});

const currentServicesTotal = computed(() => {
  if (appState.shareModule === "access") return filteredAccessItems.value.length;
  if (appState.shareTertiary === "share-review") return filteredShareReviewApplications.value.length;
  if (appState.shareTertiary === "shared-data") return filteredSharedDataItems.value.length;
  if (appState.shareTertiary === "share-calls") return filteredSharedCallItems.value.length;
  return filteredShareApplications.value.length;
});

const currentServicesPage = computed(() => {
  if (appState.shareModule === "access") return appState.accessPage;
  if (appState.shareTertiary === "share-review") return appState.shareReviewPage;
  if (appState.shareTertiary === "shared-data") return appState.sharedDataPage;
  if (appState.shareTertiary === "share-calls") return appState.sharedCallPage;
  return appState.shareListPage;
});

const currentServicesPageCount = computed(() => Math.max(1, Math.ceil(currentServicesTotal.value / appState.servicesPageSize)));
const currentServicesPageStart = computed(() => (currentServicesTotal.value ? (currentServicesPage.value - 1) * appState.servicesPageSize + 1 : 0));
const currentServicesPageEnd = computed(() => Math.min(currentServicesPage.value * appState.servicesPageSize, currentServicesTotal.value));

const analysisMapSummary = computed(() => {
  const topRegion = [...analysisResourceCounts.value].sort((a, b) => b.value - a.value)[0];
  return [
    { title: "资源主体总量", value: `${resources.length} 条` },
    { title: "统计口径", value: analysisMetricOptions.find((item) => item.id === appState.analysisMetric)?.label ?? "总资源数" },
    { title: "最高地州", value: topRegion ? `${topRegion.name} · ${topRegion.value}` : "-" },
  ];
});

const analysisRankedRegions = computed(() => [...analysisResourceCounts.value].sort((a, b) => b.value - a.value));

const analysisAccessSummary = computed(() => {
  const latestAccess = accessTrend[accessTrend.length - 1];
  const latestShare = shareTrend[shareTrend.length - 1];
  return [
    { title: "接入数据表总量", value: `${dataAccessCatalog.length}` },
    { title: "共享清单总量", value: `${serviceCatalog.length}` },
    { title: "最新接入月份", value: `${latestAccess.month} · ${latestAccess.value}` },
    { title: "最新共享月份", value: `${latestShare.month} · ${latestShare.value}` },
  ];
});

const ruralTourismLevelMap = computed(() =>
  Object.fromEntries(ruralTourismBoard.levelSummary.map((item) => [item.level, item])),
);

const ruralTourismProvinceRow = computed(() => ({
  city: "全省",
  passed: ruralTourismBoard.provinceTotals.passed,
  failed: ruralTourismBoard.provinceTotals.failed,
  inProgress: ruralTourismBoard.provinceTotals.inProgress,
  total: ruralTourismBoard.provinceTotals.total,
  levels: ruralTourismLevelMap.value,
}));

const ruralTourismTopCity = computed(() => [...ruralTourismBoard.citySummary].sort((a, b) => b.total - a.total)[0]);
const ruralTourismMaxCityTotal = computed(() => Math.max(...ruralTourismBoard.citySummary.map((item) => item.total), 1));

const ruralTourismLevelReportPanels = computed(() =>
  ruralTourismBoard.levelSummary.map((levelItem) => {
    const rows = ruralTourismBoard.citySummary.map((cityItem) => ({
      city: cityItem.city,
      ...cityItem.levels[levelItem.level],
    }));
    return {
      level: levelItem.level,
      max: Math.max(...rows.map((row) => row.total), 1),
      rows,
    };
  }),
);

const analysisBusinessSummary = computed(() => [
  { title: "村寨申报总量", value: `${ruralTourismBoard.provinceTotals.total}` },
  { title: "通过总量", value: `${ruralTourismBoard.provinceTotals.passed}` },
  { title: "不通过总量", value: `${ruralTourismBoard.provinceTotals.failed}` },
  { title: "进行中", value: `${ruralTourismBoard.provinceTotals.inProgress}` },
]);

const analysisSummaryCards = computed(() => {
  if (appState.analysisModule === "map") return analysisMapSummary.value;
  if (appState.analysisModule === "business") return analysisBusinessSummary.value;
  return analysisAccessSummary.value;
});

const sharedServiceGroups = computed(() => {
  const order = serviceTypes.filter((item) => item !== "全部");
  return order
    .map((type) => ({
      type,
      items: filteredServices.value.filter((item) => item.type === type),
    }))
    .filter((group) => group.items.length);
});

const analysisResourceCounts = computed(() => {
  const scopedResources =
    appState.analysisMetric === "total" ? resources : resources.filter((item) => item.category === appState.analysisMetric);
  return guizhouMapRegions.map((region) => ({
    ...region,
    value: scopedResources.filter((item) => item.city === region.city).length,
    dimension: appState.analysisMetric === "total" ? "总资源数" : appState.analysisMetric,
  }));
});

const analysisMapMax = computed(() => Math.max(...analysisResourceCounts.value.map((item) => item.value), 1));
const accessTrendMax = computed(() => Math.max(...accessTrend.map((item) => item.value), 1));
const shareTrendMax = computed(() => Math.max(...shareTrend.map((item) => item.value), 1));

const shareTypeDistribution = computed(() => {
  return serviceTypes
    .filter((item) => item !== "全部")
    .map((type) => ({ label: type, value: serviceCatalog.filter((item) => item.type === type).length }))
    .filter((item) => item.value > 0);
});

const shareUnitDistribution = computed(() => {
  const targetBuckets = [
    { label: "厅机关", match: "厅机关" },
    { label: "市州区县", match: "市州区县" },
    { label: "景区单位", match: "景区" },
    { label: "业务处室", match: "处" },
    { label: "外部应用", match: "系统" },
  ];
  return targetBuckets
    .map((bucket) => ({
      label: bucket.label,
      value: serviceCatalog.filter((item) => item.target.includes(bucket.match)).length,
    }))
    .filter((item) => item.value > 0);
});

const shareTypeSegments = computed(() => ringSegments(shareTypeDistribution.value));
const shareUnitSegments = computed(() => ringSegments(shareUnitDistribution.value));

const governanceSummary = [
  { title: "资源管理", value: "统一主数据" },
  { title: "业务系统目录", value: "统一来源管理" },
  { title: "数据资产中心", value: "统一沉淀复用" },
  { title: "数据接入与共享", value: "统一服务出口" },
];

function getQueryDraftConfig() {
  return {
    theme: appState.queryTheme,
    metricId: appState.queryMetricId,
    timePreset: appState.queryTimePreset,
    granularity: appState.queryGranularity,
    region: appState.queryRegion,
    dimension: appState.queryDimension,
    source: appState.querySource,
    resourceCategory: appState.queryResourceCategory,
    levelFilter: appState.queryLevelFilter,
    chargeFilter: appState.queryChargeFilter,
  };
}

function normalizeQueryConfig(config = {}) {
  return { ...defaultQueryConfig, ...config };
}

function applyQueryConfigToDraft(config) {
  const normalized = normalizeQueryConfig(config);
  appState.queryTheme = normalized.theme;
  appState.queryMetricId = normalized.metricId;
  appState.queryTimePreset = normalized.timePreset;
  appState.queryGranularity = normalized.granularity;
  appState.queryRegion = normalized.region;
  appState.queryDimension = normalized.dimension;
  appState.querySource = normalized.source;
  appState.queryResourceCategory = normalized.resourceCategory;
  appState.queryLevelFilter = normalized.levelFilter;
  appState.queryChargeFilter = normalized.chargeFilter;
  syncQueryControls();
}

function setExecutedQueryConfig(config = getQueryDraftConfig()) {
  Object.assign(appState.queryExecuted, normalizeQueryConfig(config));
}

function executeQuery(message) {
  syncQueryControls();
  setExecutedQueryConfig();
  const metric = queryMetricCatalog.find((item) => item.id === appState.queryExecuted.metricId) ?? activeQueryMetric.value;
  queryActionMessage.value =
    message ?? `已按当前条件完成查询：${metric.label} / ${appState.queryExecuted.region} / ${appState.queryExecuted.dimension}。`;
}

function syncQueryControls() {
  let metric = activeQueryMetric.value ?? queryMetricCatalog[0];
  const availableMetrics = queryMetricOptions.value;
  if (!availableMetrics.some((item) => item.id === appState.queryMetricId)) {
    appState.queryMetricId = availableMetrics[0]?.id ?? queryMetricCatalog[0].id;
    metric = queryMetricCatalog.find((item) => item.id === appState.queryMetricId) ?? queryMetricCatalog[0];
  }
  const metricGranularity = metric?.granularityOptions ?? ["日"];
  const availableGranularity = queryGranularityOptions.value;
  if (!availableGranularity.includes(appState.queryGranularity)) {
    appState.queryGranularity = availableGranularity[0] ?? metricGranularity[0];
  }
  const metricSources = metric?.sources ?? ["综合口径"];
  if (appState.querySource !== "全部" && !metricSources.includes(appState.querySource)) {
    appState.querySource = metricSources[0] ?? "全部";
  }
  if ((metric.sources?.length ?? 0) <= 1) {
    appState.querySource = metricSources[0] ?? "综合口径";
  }
  const metricDimensions = metric?.dimensions ?? ["市州"];
  const availableDimensions = queryDimensionOptions.value;
  if (!availableDimensions.includes(appState.queryDimension)) {
    const fallback = queryDimensionFallback[metric?.theme] ?? metricDimensions[0] ?? "市州";
    appState.queryDimension = availableDimensions.includes(fallback) ? fallback : availableDimensions[0] ?? "市州";
  }
  if (metric.mode === "resource") {
    appState.queryGranularity = "当前";
  }
  if (metric.theme !== "resource") {
    appState.queryResourceCategory = "全部";
    appState.queryLevelFilter = "全部";
    appState.queryChargeFilter = "全部";
  }
  if (metric.theme === "foundation") {
    appState.queryRegion = "全省";
  }
}

function selectQueryTheme(themeId) {
  appState.queryTheme = themeId;
  const targetMetricId = queryThemeCatalog.find((item) => item.id === themeId)?.metrics?.[0];
  if (targetMetricId) {
    appState.queryMetricId = targetMetricId;
  }
  syncQueryControls();
}

function selectQueryMetric(metricId) {
  appState.queryMetricId = metricId;
  syncQueryControls();
}

function selectQueryTimePreset(timePreset) {
  appState.queryTimePreset = timePreset;
  syncQueryControls();
}

function selectQueryGranularity(granularity) {
  appState.queryGranularity = granularity;
  syncQueryControls();
}

function selectQueryResultView(viewId) {
  appState.queryResultView = viewId;
}

function selectQueryModule(moduleId, queryConfig) {
  const dedicatedMeta = queryDedicatedModuleMeta[moduleId];
  if (dedicatedMeta) {
    applyQueryConfigToDraft(queryConfig ?? dedicatedMeta.defaultConfig);
    appState.queryResultView = "trend";
    executeQuery(queryConfig ? `已进入${dedicatedMeta.eyebrow}详情页，并完成专题查询。` : `已进入${dedicatedMeta.eyebrow}，并按默认条件完成查询。`);
  } else if (queryConfig) {
    applyQueryConfigToDraft(queryConfig);
    appState.queryResultView = "trend";
    executeQuery("已按专题报表条件完成查询。");
  }
  appState.queryModule = moduleId;
  appState.view = "query";
}

function resetQueryFilters() {
  applyQueryConfigToDraft(defaultQueryConfig);
  appState.queryResultView = "trend";
  executeQuery("已恢复默认查询条件并完成查询。");
}

function applyQueryReportTemplate(templateId) {
  const template = queryReportTemplates.find((item) => item.id === templateId);
  if (!template) return;
  const templateConfig = {
    theme: template.theme,
    metricId: template.metricId,
    timePreset: template.timePreset,
    granularity: template.granularity,
    region: template.region,
    dimension: template.dimension,
    source: template.source,
  };
  selectQueryModule(template.queryModule ?? "query-explore", templateConfig);
  executeQuery(`已载入专题报表“${template.title}”并完成查询。`);
}

function saveCurrentQuery() {
  executeQuery("正在保存当前查询。");
  const executed = normalizeQueryConfig(appState.queryExecuted);
  const duplicate = savedQueryItems.find(
    (item) =>
      item.metricId === executed.metricId &&
      item.theme === executed.theme &&
      item.timePreset === executed.timePreset &&
      item.granularity === executed.granularity &&
      item.region === executed.region &&
      item.dimension === executed.dimension &&
      item.source === executed.source,
  );

  if (duplicate) {
    duplicate.updatedAt = "2026-07-11 10:20";
    duplicate.summary = currentQueryResult.value.summaryText;
    queryActionMessage.value = `已更新常用查询“${duplicate.name}”。`;
    return;
  }

  savedQueryItems.unshift({
    id: `saved-${savedQueryItems.length + 1}`,
    name: `${executedQueryMetric.value.label} · ${executed.region}`,
    theme: executed.theme,
    metricId: executed.metricId,
    timePreset: executed.timePreset,
    granularity: executed.granularity,
    region: executed.region,
    dimension: executed.dimension,
    source: executed.source,
    updatedAt: "2026-07-11 10:20",
    summary: currentQueryResult.value.summaryText,
  });
  queryActionMessage.value = "当前查询已保存到“我的查询”。";
}

function runSavedQuery(id) {
  const item = savedQueryItems.find((entry) => entry.id === id);
  if (!item) return;
  applyQueryConfigToDraft(item);
  appState.queryModule = "query-explore";
  appState.view = "query";
  executeQuery(`已载入常用查询“${item.name}”并完成查询。`);
}

function removeSavedQuery(id) {
  const index = savedQueryItems.findIndex((item) => item.id === id);
  if (index < 0) return;
  const [removed] = savedQueryItems.splice(index, 1);
  queryActionMessage.value = `已移除常用查询“${removed.name}”。`;
}

function exportCurrentQuery() {
  const columns = currentQueryTable.value.columns ?? [];
  const rows = currentQueryTable.value.rows ?? [];
  if (!columns.length || !rows.length) {
    queryActionMessage.value = "当前结果暂无可导出数据。";
    return;
  }

  const header = columns.map((item) => item.label).join(",");
  const body = rows
    .map((row) =>
      columns
        .map((column) => {
          const value = row[column.key] ?? "";
          return `"${String(value).replaceAll('"', '""')}"`;
        })
        .join(","),
    )
    .join("\n");

  const csv = `${header}\n${body}`;
  const blob = new Blob([`\uFEFF${csv}`], { type: "text/csv;charset=utf-8;" });
  const link = document.createElement("a");
  link.href = URL.createObjectURL(blob);
  link.download = `${executedQueryMetric.value.label}_${appState.queryExecuted.region}_${appState.queryExecuted.granularity}.csv`;
  link.click();
  URL.revokeObjectURL(link.href);
  queryActionMessage.value = "已导出当前查询结果。";
}

function openQueryMetricDirectory() {
  if (!currentQueryRelatedMetric.value) {
    queryActionMessage.value = "当前指标暂无关联的指标资产条目。";
    return;
  }
  openMetricDetail(currentQueryRelatedMetric.value.id);
}

function openResourceFromQuery(resourceId) {
  if (!resourceId) return;
  openResourceDetail(resourceId);
}

function findAssetsForQueryMetric(metric) {
  if (!metric) return [];
  const keywordMap = {
    resource: ["主数据", "资源"],
    flow: ["客流", "LBS", "景区"],
    operation: ["客流", "消费", "服务", "专题"],
    stay: ["住宿", "酒店", "PMS"],
    consume: ["消费", "活动", "标签"],
    emergency: ["安全", "视频", "气象"],
    foundation: ["指标", "服务", "知识库", "共享"],
  };
  const keywords = keywordMap[metric.theme] ?? [metric.label];
  return dataAssets.filter((item) => keywords.some((keyword) => item.name.includes(keyword) || item.serviceScenes.includes(keyword)));
}

function buildQueryResult(metric, queryConfig = appState.queryExecuted) {
  if (!metric) return { summaryCards: [], trendPoints: [], distributionRows: [], matrix: { columns: [], rows: [] }, table: { columns: [], rows: [] }, summaryText: "" };
  const config = normalizeQueryConfig(queryConfig);
  if (metric.mode === "resource") {
    return buildResourceQueryResult(metric, config);
  }
  return buildTimeMetricQueryResult(metric, config);
}

function buildResourceQueryResult(metric, queryConfig) {
  const matchedResources = filterResourcesForQuery(metric, queryConfig);
  const distributionRows = buildResourceDistributionRows(metric, matchedResources, queryConfig);
  const summaryCards = [
    { title: "当前值", value: formatQueryValue(matchedResources.length, metric.unit), meta: "统一底账口径" },
    { title: "覆盖市州", value: `${new Set(matchedResources.map((item) => item.city)).size} 个`, meta: "覆盖范围" },
    { title: "高等级资源", value: `${matchedResources.filter((item) => /5A|4A|重点|一级/.test(item.level)).length} 个`, meta: "重点资源数量" },
    { title: "可查看明细", value: `${matchedResources.length} 条`, meta: "支持导出明细" },
  ];
  const trendPoints = buildResourceTrendPoints(metric, matchedResources.length);
  const matrix = buildResourceMatrix(metric, queryConfig, matchedResources);
  const table = {
    columns: [
      { key: "name", label: "资源名称" },
      { key: "category", label: "资源类型" },
      { key: "level", label: "等级" },
      { key: "city", label: "市州" },
      { key: "county", label: "区县" },
      { key: "chargeMode", label: "收费情况" },
      { key: "actionLabel", label: "操作" },
    ],
    rows: matchedResources.slice(0, 20).map((item) => ({
      name: item.name,
      category: item.category,
      level: item.level,
      city: item.city,
      county: item.county,
      chargeMode: item.chargeMode,
      actionLabel: "查看资源",
      _resourceId: item.id,
    })),
  };

  return {
    summaryCards,
    trendPoints,
    distributionRows,
    matrix,
    table,
    summaryText: `${metric.label}当前共 ${matchedResources.length} 个，支持按${queryConfig.dimension}等维度查询。`,
  };
}

function buildTimeMetricQueryResult(metric, queryConfig) {
  const trendPoints = buildTimeMetricTrendPoints(metric, queryConfig);
  const distributionRows = buildTimeMetricDistributionRows(metric, trendPoints, queryConfig);
  const total = trendPoints.reduce((sum, item) => sum + item.value, 0);
  const peakPoint = [...trendPoints].sort((a, b) => b.value - a.value)[0] ?? trendPoints[0];
  const avg = trendPoints.length ? total / trendPoints.length : 0;
  const summaryCards = [
    { title: "累计值", value: formatQueryValue(total, metric.unit), meta: `${queryConfig.granularity}粒度汇总` },
    { title: "平均值", value: formatQueryValue(avg, metric.unit), meta: "平均水平" },
    { title: "峰值", value: peakPoint ? formatQueryValue(peakPoint.value, metric.unit) : "-", meta: peakPoint?.label ?? "无峰值" },
    { title: "统计范围", value: queryConfig.region, meta: queryConfig.source === "全部" ? "默认综合口径" : queryConfig.source },
  ];
  const matrix = buildTimeMetricMatrix(metric, queryConfig);
  const table = {
    columns: [
      { key: "period", label: queryPeriodColumnLabel(queryConfig.granularity) },
      { key: "region", label: "统计范围" },
      { key: "value", label: metric.label },
      { key: "source", label: "数据来源" },
      { key: "note", label: "口径说明" },
    ],
    rows: trendPoints.map((item) => ({
      period: item.label,
      region: queryConfig.region,
      value: formatQueryValue(item.value, metric.unit),
      source: queryConfig.source === "全部" ? "综合口径" : queryConfig.source,
      note: metric.definition,
    })),
  };

  return {
    summaryCards,
    trendPoints,
    distributionRows,
    matrix,
    table,
    summaryText: `${metric.label}支持按${queryConfig.dimension}等维度进行组合查询，当前范围为${queryConfig.region}。`,
  };
}

function queryPeriodColumnLabel(granularity) {
  if (granularity === "小时") return "小时";
  if (granularity === "日") return "日期";
  if (granularity === "周") return "周次";
  if (granularity === "月") return "月份";
  if (granularity === "季度") return "季度";
  if (granularity === "半年") return "半年";
  if (granularity === "年") return "年份";
  if (granularity === "节假日") return "节假日";
  return "时间";
}

function filterResourcesForQuery(metric, queryConfig) {
  let matchedResources = resources.filter((item) => !metric.resourceCategories || metric.resourceCategories.includes(item.category));
  if (metric.id === "resource_total" && queryConfig.resourceCategory !== "全部") {
    matchedResources = matchedResources.filter((item) => item.category === queryConfig.resourceCategory);
  }
  if (queryConfig.region !== "全省") {
    matchedResources = matchedResources.filter((item) => item.city === queryConfig.region);
  }
  if (queryConfig.levelFilter !== "全部") {
    matchedResources = matchedResources.filter((item) => item.level === queryConfig.levelFilter);
  }
  if (queryConfig.chargeFilter !== "全部") {
    matchedResources = matchedResources.filter((item) => item.chargeMode === queryConfig.chargeFilter);
  }
  return matchedResources;
}

function buildResourceDistributionRows(metric, matchedResources, queryConfig) {
  const buckets = new Map();
  matchedResources.forEach((item) => {
    const key = resourceDimensionValue(item, queryConfig.dimension || metric.dimensions[0]);
    buckets.set(key, (buckets.get(key) ?? 0) + 1);
  });
  return [...buckets.entries()]
    .map(([label, value]) => ({
      label,
      value,
      displayValue: formatQueryValue(value, metric.unit),
      ratio: matchedResources.length ? `${((value / matchedResources.length) * 100).toFixed(1)}%` : "0.0%",
    }))
    .sort((a, b) => b.value - a.value);
}

function resourceDimensionValue(item, dimension) {
  if (dimension === "市州") return item.city;
  if (dimension === "区县") return item.county;
  if (dimension === "景区") return item.category === "景区" ? item.name : "非景区资源";
  if (dimension === "资源类型" || dimension === "主体类型" || dimension === "场馆类型") return item.category;
  if (dimension === "等级") return item.level;
  if (dimension === "收费情况") return item.chargeMode;
  if (dimension === "预约情况") return findResourceField(item, ["是否预约"]) || "未配置";
  if (dimension === "经营状态") return findResourceField(item, ["营业状态", "当前经营状态"]) || "正常";
  if (dimension === "OTA上线情况") return findResourceField(item, ["在哪些 OTA 平台上线"]) ? "已上线" : "未配置";
  return item.city;
}

function buildResourceTrendPoints(metric, count) {
  const factors = [0.84, 0.86, 0.89, 0.92, 0.95, 0.98, 1];
  return factors.map((factor, index) => ({
    label: `${index + 1}月`,
    value: Math.round(count * factor),
    displayValue: formatQueryValue(Math.round(count * factor), metric.unit),
  }));
}

function buildMetricMonthlyBase(metric) {
  const base = metric.monthlyBase?.length ? [...metric.monthlyBase] : [0];
  if (base.length >= 12) return base.slice(0, 12);
  const extended = [...base];
  while (extended.length < 12) {
    const last = extended[extended.length - 1] ?? 0;
    const seasonFactor = 0.96 + ((extended.length + 2) % 4) * 0.035;
    extended.push(last * seasonFactor);
  }
  return extended;
}

function buildTimeBucketPoints(metric, queryConfig, totalFactor) {
  const monthlyBase = buildMetricMonthlyBase(metric);
  const monthIndex = queryConfig.timePreset === "recent-7" ? 6 : queryConfig.timePreset === "month-2026-07" ? 6 : 6;
  const monthValue = (monthlyBase[monthIndex] ?? monthlyBase[0] ?? 0) * totalFactor;

  if (queryConfig.granularity === "年") {
    const value = monthlyBase.reduce((sum, item) => sum + item, 0) * totalFactor;
    return [{ label: "2026年", value, displayValue: formatQueryValue(value, metric.unit) }];
  }

  if (queryConfig.granularity === "半年") {
    const buckets = [
      { label: "上半年", indexes: [0, 1, 2, 3, 4, 5] },
      { label: "下半年", indexes: [6, 7, 8, 9, 10, 11] },
    ];
    return buckets.map((bucket) => {
      const value = bucket.indexes.reduce((sum, index) => sum + (monthlyBase[index] ?? 0), 0) * totalFactor;
      return { label: bucket.label, value, displayValue: formatQueryValue(value, metric.unit) };
    });
  }

  if (queryConfig.granularity === "季度") {
    const buckets = [
      { label: "一季度", indexes: [0, 1, 2] },
      { label: "二季度", indexes: [3, 4, 5] },
      { label: "三季度", indexes: [6, 7, 8] },
      { label: "四季度", indexes: [9, 10, 11] },
    ];
    return buckets.map((bucket) => {
      const value = bucket.indexes.reduce((sum, index) => sum + (monthlyBase[index] ?? 0), 0) * totalFactor;
      return { label: bucket.label, value, displayValue: formatQueryValue(value, metric.unit) };
    });
  }

  if (queryConfig.granularity === "月") {
    const monthIndexes = queryConfig.timePreset === "month-2026-07" ? [6] : monthlyBase.map((_, index) => index);
    return monthIndexes.map((monthIndex) => {
      const value = (monthlyBase[monthIndex] ?? 0) * totalFactor;
      return {
        label: `${monthIndex + 1}月`,
        value,
        displayValue: formatQueryValue(value, metric.unit),
      };
    });
  }

  if (queryConfig.granularity === "周") {
    const weekWeights = queryConfig.timePreset === "recent-7" ? [1] : [0.22, 0.24, 0.26, 0.28];
    return weekWeights.map((weight, index) => {
      const value = monthValue * weight;
      return { label: queryConfig.timePreset === "recent-7" ? "近7日" : `第${index + 1}周`, value, displayValue: formatQueryValue(value, metric.unit) };
    });
  }

  if (queryConfig.granularity === "小时") {
    const hourlyWeights = Array.from({ length: 24 }, (_, index) => (index >= 8 && index <= 21 ? 0.76 + ((index + 3) % 6) * 0.08 : 0.18));
    const hourlyTotal = hourlyWeights.reduce((sum, item) => sum + item, 0) || 1;
    return hourlyWeights.map((weight, index) => {
      const value = monthValue * (weight / hourlyTotal);
      return { label: `${String(index).padStart(2, "0")}时`, value, displayValue: formatQueryValue(value, metric.unit) };
    });
  }

  if (queryConfig.granularity === "节假日" || queryConfig.timePreset === "holiday-2026") {
    const holidayBuckets = [
      { label: "春节", weight: 0.28 },
      { label: "清明", weight: 0.08 },
      { label: "五一", weight: 0.18 },
      { label: "端午", weight: 0.1 },
      { label: "暑期", weight: 0.2 },
      { label: "国庆", weight: 0.16 },
    ];
    const yearValue = monthlyBase.reduce((sum, item) => sum + item, 0) * totalFactor;
    return holidayBuckets.map((item) => {
      const value = yearValue * item.weight;
      return { label: item.label, value, displayValue: formatQueryValue(value, metric.unit) };
    });
  }

  const dayCount = queryConfig.timePreset === "recent-7" ? 7 : 31;
  const dailyWeights = buildDailyWeights(dayCount);
  return dailyWeights.map((weight, index) => {
    const day = queryConfig.timePreset === "recent-7" ? index + 5 : index + 1;
    const value = monthValue * weight;
    return {
      label: `07-${String(day).padStart(2, "0")}`,
      value,
      displayValue: formatQueryValue(value, metric.unit),
    };
  });
}

function buildResourceMatrix(metric, queryConfig, matchedResources) {
  const rowDimension = queryConfig.dimension || "市州";
  const columnDimension = resourceMatrixColumnDimension(metric, rowDimension);
  const rowLabels = resourceMatrixLabels(matchedResources, rowDimension).slice(0, rowDimension === "景区" ? 12 : 18);
  const columnLabels = resourceMatrixLabels(matchedResources, columnDimension).slice(0, 12);
  const rows = rowLabels.map((rowLabel) => {
    const rowResources = matchedResources.filter((item) => resourceDimensionValue(item, rowDimension) === rowLabel);
    const cells = columnLabels.map(
      (columnLabel) => rowResources.filter((item) => resourceDimensionValue(item, columnDimension) === columnLabel).length,
    );
    return { label: rowLabel, cells, total: cells.reduce((sum, item) => sum + item, 0) };
  });
  const sortedRows = rows.sort((a, b) => b.total - a.total);
  return {
    rowHeader: rowDimension,
    columnHeader: columnDimension,
    columns: columnLabels,
    rows: sortedRows,
  };
}

function resourceMatrixColumnDimension(metric, rowDimension) {
  const defaultByMetric = {
    resource_total: "资源类型",
    scenic_count: "等级",
    hotel_count: "经营状态",
    cultural_venue_count: "场馆类型",
    travel_subject_count: "主体类型",
  };
  if (["市州", "区县"].includes(rowDimension)) return defaultByMetric[metric.id] ?? "资源类型";
  if (rowDimension === "景区") return metric.id === "scenic_count" ? "收费情况" : "资源类型";
  if (["资源类型", "主体类型", "场馆类型"].includes(rowDimension)) return "市州";
  if (rowDimension === "等级") return metric.id === "resource_total" ? "资源类型" : "市州";
  if (["收费情况", "预约情况", "经营状态", "OTA上线情况"].includes(rowDimension)) return "市州";
  return "市州";
}

function resourceMatrixLabels(items, dimension) {
  const labels = [...new Set(items.map((item) => resourceDimensionValue(item, dimension)).filter(Boolean))];
  return labels.sort((a, b) => {
    const countA = items.filter((item) => resourceDimensionValue(item, dimension) === a).length;
    const countB = items.filter((item) => resourceDimensionValue(item, dimension) === b).length;
    return countB - countA || String(a).localeCompare(String(b), "zh-CN");
  });
}

function buildTimeMetricTrendPoints(metric, queryConfig) {
  const scenicFactor = queryScenicWeightMap[queryConfig.region];
  const regionFactor = queryConfig.region === "全省" ? 1 : scenicFactor ? scenicFactor : (queryCityWeightMap[queryConfig.region] ?? 0.12) * 1.08;
  const sourceFactor = querySourceFactorMap[queryConfig.source === "全部" ? metric.sources[0] : queryConfig.source] ?? 1;
  const resourceFactor = queryResourceTypeFactorMap[queryConfig.resourceCategory] ?? 1;
  const levelFactor = queryConfig.levelFilter === "全部" ? 1 : /5A|4A/.test(queryConfig.levelFilter) ? 0.38 : 0.66;
  const chargeFactor = queryConfig.chargeFilter === "全部" ? 1 : queryConfig.chargeFilter === "收费" ? 0.76 : 0.24;
  const totalFactor = regionFactor * sourceFactor * resourceFactor * levelFactor * chargeFactor;
  return buildTimeBucketPoints(metric, queryConfig, totalFactor);
}

function buildDailyWeights(length) {
  const raw = Array.from({ length }, (_, index) => 0.86 + ((index + 2) % 7) * 0.045 + (index % 5 === 0 ? 0.12 : 0));
  const total = raw.reduce((sum, item) => sum + item, 0);
  return raw.map((item) => item / total);
}

function buildTimeMetricDistributionRows(metric, trendPoints, queryConfig) {
  const total = trendPoints.reduce((sum, item) => sum + item.value, 0) || 1;
  if (queryConfig.dimension === "月份") {
    return buildTimeMetricTrendPoints({ ...metric, monthlyBase: metric.monthlyBase, unit: metric.unit }, { ...queryConfig, granularity: "月", timePreset: "year-2026" }).map((item) => ({
      label: item.label,
      value: item.value,
      displayValue: formatQueryValue(item.value, metric.unit),
      ratio: `${((item.value / (buildMetricMonthlyBase(metric).reduce((sum, value) => sum + value, 0) || 1)) * 100).toFixed(1)}%`,
    }));
  }

  const dimensionRows = buildDimensionValueRows(metric, total, queryConfig);
  return dimensionRows.map((item) => ({
    ...item,
    displayValue: formatQueryValue(item.value, metric.unit),
    ratio: `${((item.value / total) * 100).toFixed(1)}%`,
  }));
}

function buildDimensionValueRows(metric, total, queryConfig) {
  if (queryConfig.dimension === "市州") {
    const scopedResource = resources.find((item) => item.name === queryConfig.region || item.county === queryConfig.region);
    const cities = queryConfig.region === "全省" ? queryCityOptions : [scopedResource?.city ?? queryConfig.region];
    return cities
      .map((city) => ({
        label: city,
        value: total * (queryConfig.region === "全省" ? queryCityWeightMap[city] ?? 0 : 1),
      }))
      .sort((a, b) => b.value - a.value);
  }

  if (queryConfig.dimension === "区县") {
    const countyResources = queryConfig.region === "全省"
      ? resources
      : resources.filter((item) => item.city === queryConfig.region || item.county === queryConfig.region);
    const countyCounts = new Map();
    countyResources.forEach((item) => countyCounts.set(item.county, (countyCounts.get(item.county) ?? 0) + 1));
    const denominator = [...countyCounts.values()].reduce((sum, value) => sum + value, 0) || 1;
    return [...countyCounts.entries()]
      .map(([label, count]) => ({ label, value: total * (count / denominator) }))
      .sort((a, b) => b.value - a.value)
      .slice(0, 12);
  }

  if (queryConfig.dimension === "景区") {
    const scenicResources = resources.filter((item) => {
      if (item.category !== "景区") return false;
      if (queryConfig.region === "全省") return true;
      return item.city === queryConfig.region || item.name === queryConfig.region;
    });
    const denominator = scenicResources.length || 1;
    return scenicResources.slice(0, 12).map((item, index) => ({
      label: item.name,
      value: total * ((denominator - index) / ((denominator * (denominator + 1)) / 2 || 1)),
    }));
  }

  if (queryConfig.dimension === "来源渠道") {
    const sources = metric.sources;
    const sourceTotal = sources.reduce((sum, source) => sum + (querySourceFactorMap[source] ?? 1), 0) || 1;
    return sources.map((source) => ({ label: source, value: total * ((querySourceFactorMap[source] ?? 1) / sourceTotal) }));
  }

  if (queryConfig.dimension === "区域层级") {
    return [
      { label: "省", value: total * 0.45 },
      { label: "市州", value: total * 0.34 },
      { label: "区县", value: total * 0.21 },
    ];
  }

  if (queryConfig.dimension === "性别") {
    return [
      { label: "男", value: total * 0.52 },
      { label: "女", value: total * 0.48 },
    ];
  }

  if (queryConfig.dimension === "年龄") {
    return [
      { label: "18-24岁", value: total * 0.18 },
      { label: "25-34岁", value: total * 0.31 },
      { label: "35-44岁", value: total * 0.24 },
      { label: "45-59岁", value: total * 0.19 },
      { label: "60岁以上", value: total * 0.08 },
    ];
  }

  if (queryConfig.dimension === "出行方式") {
    return [
      { label: "自驾", value: total * 0.42 },
      { label: "高铁", value: total * 0.22 },
      { label: "航空", value: total * 0.14 },
      { label: "团队大巴", value: total * 0.12 },
      { label: "公共交通", value: total * 0.1 },
    ];
  }

  if (queryConfig.dimension === "游客停留时长") {
    return [
      { label: "2小时内", value: total * 0.16 },
      { label: "半日", value: total * 0.28 },
      { label: "1日", value: total * 0.34 },
      { label: "2日及以上", value: total * 0.22 },
    ];
  }

  if (queryConfig.dimension === "游客来源地") {
    return [
      { label: "省内", value: total * 0.36 },
      { label: "周边省份", value: total * 0.24 },
      { label: "长三角", value: total * 0.16 },
      { label: "珠三角", value: total * 0.11 },
      { label: "京津冀", value: total * 0.07 },
      { label: "其他", value: total * 0.06 },
    ];
  }

  if (queryConfig.dimension === "旅游半径") {
    return [
      { label: "50公里内", value: total * 0.24 },
      { label: "50-150公里", value: total * 0.29 },
      { label: "150-500公里", value: total * 0.27 },
      { label: "500公里以上", value: total * 0.2 },
    ];
  }

  if (queryConfig.dimension === "夜游情况") {
    return [
      { label: "夜游", value: total * 0.38 },
      { label: "非夜游", value: total * 0.62 },
    ];
  }

  if (queryConfig.dimension === "平均旅游城市") {
    return [
      { label: "单城", value: total * 0.48 },
      { label: "双城", value: total * 0.34 },
      { label: "三城及以上", value: total * 0.18 },
    ];
  }

  if (queryConfig.dimension === "接待类型") {
    return [
      { label: "总客流", value: total * 0.46 },
      { label: "实时在园", value: total * 0.22 },
      { label: "日累计接待", value: total * 0.32 },
    ];
  }

  if (queryConfig.dimension === "预约渠道") {
    return [
      { label: "官方平台", value: total * 0.34 },
      { label: "OTA", value: total * 0.28 },
      { label: "现场扫码", value: total * 0.22 },
      { label: "团队预约", value: total * 0.16 },
    ];
  }

  if (queryConfig.dimension === "资源类型") {
    return ["景区", "街区", "度假区", "乡村旅游点", "酒店民宿"].map((type) => ({
      label: type,
      value: total * (queryResourceTypeFactorMap[type] ?? 0.2),
    }));
  }

  if (queryConfig.dimension === "渠道") {
    return [
      { label: "OTA", value: total * 0.38 },
      { label: "官网", value: total * 0.21 },
      { label: "现场", value: total * 0.27 },
      { label: "旅行社", value: total * 0.14 },
    ];
  }

  if (queryConfig.dimension === "景区等级" || queryConfig.dimension === "等级") {
    return [
      { label: "5A级", value: total * 0.23 },
      { label: "4A级", value: total * 0.34 },
      { label: "3A级", value: total * 0.27 },
      { label: "其他", value: total * 0.16 },
    ];
  }

  if (queryConfig.dimension === "住宿类型") {
    return [
      { label: "酒店", value: total * 0.62 },
      { label: "民宿", value: total * 0.28 },
      { label: "客栈", value: total * 0.1 },
    ];
  }

  if (queryConfig.dimension === "风险等级") {
    return [
      { label: "高风险", value: total * 0.18 },
      { label: "中风险", value: total * 0.36 },
      { label: "低风险", value: total * 0.46 },
    ];
  }

  if (queryConfig.dimension === "事件类型") {
    return [
      { label: "客流拥挤", value: total * 0.32 },
      { label: "气象风险", value: total * 0.26 },
      { label: "停车异常", value: total * 0.18 },
      { label: "游客求助", value: total * 0.14 },
      { label: "设备离线", value: total * 0.1 },
    ];
  }

  if (queryConfig.dimension === "活动类型") {
    return [
      { label: "节庆活动", value: total * 0.34 },
      { label: "演艺活动", value: total * 0.24 },
      { label: "非遗体验", value: total * 0.18 },
      { label: "体育赛事", value: total * 0.14 },
      { label: "夜游活动", value: total * 0.1 },
    ];
  }

  if (queryConfig.dimension === "责任单位") {
    return [
      { label: "安全应急处", value: total * 0.36 },
      { label: "信息中心", value: total * 0.28 },
      { label: "市州文旅单位", value: total * 0.22 },
      { label: "景区运营单位", value: total * 0.14 },
    ];
  }

  if (queryConfig.dimension === "共享类型") {
    return [
      { label: "数据指标", value: total * 0.34 },
      { label: "主题数据集", value: total * 0.42 },
      { label: "旅游资源", value: total * 0.24 },
    ];
  }

  if (queryConfig.dimension === "数据类型") {
    return [
      { label: "旅游资源数据", value: total * 0.2 },
      { label: "LBS数据", value: total * 0.24 },
      { label: "消费数据", value: total * 0.14 },
      { label: "视频数据", value: total * 0.16 },
      { label: "知识数据", value: total * 0.12 },
      { label: "订单数据", value: total * 0.14 },
    ];
  }

  if (queryConfig.dimension === "应用场景") {
    return [
      { label: "政府智能体", value: total * 0.42 },
      { label: "黄小西智能体", value: total * 0.38 },
      { label: "酒店智能体", value: total * 0.2 },
    ];
  }

  return [{ label: queryConfig.dimension, value: total }];
}

function buildTimeMetricMatrix(metric, queryConfig) {
  const columns = buildTimeMetricTrendPoints(metric, queryConfig).map((item) => item.label);
  const normalizedColumns = columns.length > 16 ? columns.filter((_, index) => index % 2 === 0) : columns;
  if (metric.theme === "foundation") {
    const rows = ["数据接入", "共享开通", "指标复用", "智能问数"].map((label, rowIndex) => ({
      label,
      cells: normalizedColumns.map((_, columnIndex) => {
        const base = buildMetricMonthlyBase(metric)[columnIndex] ?? buildMetricMonthlyBase(metric)[0] ?? 0;
        return Math.round(base * (0.78 + rowIndex * 0.08));
      }),
      total: normalizedColumns.reduce((sum, _, columnIndex) => {
        const base = buildMetricMonthlyBase(metric)[columnIndex] ?? buildMetricMonthlyBase(metric)[0] ?? 0;
        return sum + Math.round(base * (0.78 + rowIndex * 0.08));
      }, 0),
    }));
    return { columns: normalizedColumns, rows };
  }

  const dimensionRows = buildDimensionValueRows(metric, 1, queryConfig).slice(0, 10);
  const trendPoints = buildTimeMetricTrendPoints(metric, queryConfig);
  const visibleTrendPoints = trendPoints.length > 16 ? trendPoints.filter((_, index) => index % 2 === 0) : trendPoints;
  const rows = dimensionRows.map((dimensionRow, rowIndex) => {
    const weight = dimensionRow.value || 0.1;
    const cells = visibleTrendPoints.map((point, columnIndex) => point.value * weight * (0.92 + ((rowIndex + columnIndex) % 4) * 0.035));
    return {
      label: dimensionRow.label,
      cells: cells.map((item) => metric.unit === "亿元" ? item.toFixed(2) : Math.round(item * 10) / 10),
      total: metric.unit === "亿元" ? cells.reduce((sum, item) => sum + item, 0).toFixed(2) : Math.round(cells.reduce((sum, item) => sum + item, 0) * 10) / 10,
    };
  });
  return { columns: normalizedColumns, rows };
}

function formatQueryValue(value, unit) {
  if (unit === "亿元") return `${Number(value).toFixed(2)}亿元`;
  if (unit === "万人次") return `${Number(value).toFixed(1)}万人次`;
  if (unit === "万人") return `${Number(value).toFixed(1)}万人`;
  if (unit === "万单") return `${Number(value).toFixed(1)}万单`;
  if (unit === "万笔") return `${Number(value).toFixed(1)}万笔`;
  if (unit === "%") return `${Number(value).toFixed(1)}%`;
  if (unit === "分") return `${Number(value).toFixed(1)}分`;
  if (unit === "元") return `${Math.round(Number(value))}元`;
  return `${Math.round(Number(value)).toLocaleString("zh-CN")}${unit}`;
}

syncQueryControls();

const feedback = ref("申请将进入“责任单位确认 - 数据办审核 - 服务开通”流程。");
const analysisTooltip = reactive({
  visible: false,
  x: 0,
  y: 0,
  title: "",
  meta: "",
});

function setView(view) {
  appState.view = view;
}

const activeShareTertiaryId = computed(() => appState.shareTertiary);

function openShareTertiaryNav(id) {
  appState.shareModule = "share";
  appState.shareTertiary = id;
  appState.view = "services";
}

function loginWithAccount(account = loginForm.account, password = loginForm.password) {
  const targetUser = userDirectory.find((item) => item.account === account);
  if (!targetUser) {
    loginMessage.value = "未找到该账号，请重新选择。";
    return;
  }
  if (targetUser.status !== "启用") {
    loginMessage.value = "该账号当前已停用，无法登录。";
    return;
  }
  if (targetUser.password !== password) {
    loginMessage.value = "密码不正确，请输入系统测试密码 123456。";
    return;
  }

  const loginTime = "2026-07-09 10:28";
  userDirectory.forEach((item) => {
    if (item.status !== "启用") return;
    if (item.id === targetUser.id) {
      item.loginState = "在线";
      item.currentSession = "当前会话";
      item.lastLogin = loginTime;
    } else if (item.currentSession === "当前会话") {
      item.currentSession = "活跃会话";
    }
  });

  appState.isAuthenticated = true;
  appState.currentUserId = targetUser.id;
  appState.selectedUserId = targetUser.id;

  const targetRole = roleDirectory.find((item) => item.name === targetUser.role);
  if (targetRole) {
    appState.selectedRoleId = targetRole.id;
  }

  const targetOrg = orgDirectory.find((item) => item.name === targetUser.org);
  if (targetOrg) {
    appState.selectedOrgId = targetOrg.id;
  }

  loginMessage.value = `${targetUser.name} 已登录系统。`;
}

function quickLogin(userId) {
  const targetUser = userDirectory.find((item) => item.id === userId);
  if (!targetUser) return;
  loginForm.account = targetUser.account;
  loginForm.password = targetUser.password;
  loginWithAccount(targetUser.account, targetUser.password);
}

function signOut() {
  const targetUser = currentUser.value;
  if (targetUser?.status === "启用") {
    targetUser.currentSession = "无在线会话";
    targetUser.loginState = "离线";
  }
  appState.isAuthenticated = false;
  loginForm.password = "123456";
  loginMessage.value = "您已退出登录，请重新选择账号进入系统。";
}

function openDashboardLink(item) {
  if (!item?.targetType || !item?.targetId) return;

  switch (item.targetType) {
    case "resource-detail":
      openResourceDetail(item.targetId);
      return;
    case "metric-detail":
      openMetricDetail(item.targetId);
      return;
    case "access-detail":
      openAccessDetail(item.targetId);
      return;
    case "share-detail":
      openShareDetail(item.targetId);
      return;
    case "share-review-detail":
      openShareReviewDetail(item.targetId);
      return;
    case "shared-data-detail":
      openSharedDataDetail(item.targetId);
      return;
    case "service-detail":
      openServiceDetail(item.targetId);
      return;
    default:
  }
}

function openSecondaryNav(id) {
  if (currentPrimaryNavId.value === "query") {
    selectQueryModule(id);
    return;
  }

  if (currentPrimaryNavId.value === "analysis") {
    appState.view = "analysis";
    appState.analysisModule = id;
    return;
  }

  if (currentPrimaryNavId.value === "permissions") {
    appState.view = "permissions";
    appState.permissionModule = id;
    return;
  }

  if (currentPrimaryNavId.value === "assets") {
    setAssetModule(id);
    appState.view = "assets";
    return;
  }

  switch (id) {
    case "dashboard-home":
      appState.view = "dashboard";
      return;
    case "dashboard-monitor":
      appState.view = "dashboard-monitor";
      return;
    case "dashboard-todos":
      appState.view = "dashboard-todos";
      return;
    case "dashboard-warnings":
      appState.view = "dashboard-warnings";
      return;
    case "tourism-resources":
    case "accommodation-resources":
    case "cultural-resources":
    case "tourism-subjects":
      appState.resourceGroup = id;
      if (!resourceCategories.value.includes(appState.resourceCategory)) {
        appState.resourceCategory = (resourceGroups.find((item) => item.id === id)?.categories ?? [])[0] ?? "景区";
      }
      appState.view = "resources";
      return;
    case "systems-directory":
      appState.view = "systems";
      return;
    case "metrics-directory":
      setAssetModule("asset-indicators");
      appState.view = "assets";
      return;
    case "access":
      appState.view = "services";
      appState.shareModule = "access";
      return;
    case "share":
      appState.view = "services";
      appState.shareModule = "share";
      return;
    case "governance-overview":
      appState.view = "governance";
      return;
    default:
  }
}

function selectCategory(category) {
  appState.resourceGroup = resourceGroupIdByCategory(category);
  appState.resourceCategory = category;
  if (!filteredResources.value.some((item) => item.id === appState.selectedResourceId) && filteredResources.value[0]) {
    appState.selectedResourceId = filteredResources.value[0].id;
  }
}

function selectResource(id) {
  appState.selectedResourceId = id;
}

function openResourceDetail(id) {
  appState.selectedResourceId = id;
  appState.view = "resource-detail";
}

function backToResources() {
  appState.view = "resources";
}

function selectSystemType(type) {
  appState.systemType = type;
  if (!filteredSystems.value.some((item) => item.id === appState.selectedSystemId) && filteredSystems.value[0]) {
    appState.selectedSystemId = filteredSystems.value[0].id;
  }
}

function selectAssetType(type) {
  appState.assetType = type;
  if (!filteredAssets.value.some((item) => item.id === appState.selectedAssetId) && filteredAssets.value[0]) {
    appState.selectedAssetId = filteredAssets.value[0].id;
  }
}

function setAssetModule(id) {
  if (id === "assets-directory") {
    appState.assetModule = "asset-master";
    selectAssetType("主数据资产");
    return;
  }
  if (id === "asset-datasets") {
    appState.assetModule = "asset-overview";
    return;
  }
  if (id === "metric-detail" || id === "metrics-directory" || id === "asset-indicators") {
    appState.assetModule = "asset-indicators";
    return;
  }
  if (id === "asset-overview") {
    appState.assetModule = "asset-overview";
    return;
  }
  if (assetModuleTypeMap[id]) {
    appState.assetModule = id;
    selectAssetType(assetModuleTypeMap[id]);
  }
}

function openAssetTypeOverview(type) {
  if (type === "指标资产") {
    setAssetModule("asset-indicators");
    return;
  }
  setAssetModule(assetTypeModuleMap[type] ?? "asset-master");
}

function selectMetricDomain(domain) {
  appState.metricDomain = domain;
  if (!tableMetrics.value.some((item) => item.id === appState.selectedMetricId) && tableMetrics.value[0]) {
    appState.selectedMetricId = tableMetrics.value[0].id;
  }
}

function selectServiceType(type) {
  appState.serviceType = type;
  if (!filteredServices.value.some((item) => item.id === appState.selectedServiceId) && filteredServices.value[0]) {
    appState.selectedServiceId = filteredServices.value[0].id;
  }
}

function selectShareModule(module) {
  appState.shareModule = module;
  if (module === "access") {
    if (!filteredAccessItems.value.some((item) => item.id === appState.selectedAccessId) && filteredAccessItems.value[0]) {
      appState.selectedAccessId = filteredAccessItems.value[0].id;
    }
    return;
  }
  if (!["share-list", "share-review", "shared-data", "share-calls"].includes(appState.shareTertiary)) {
    appState.shareTertiary = "share-list";
  }
  if (!filteredShareApplications.value.some((item) => item.id === appState.selectedShareApplicationId) && filteredShareApplications.value[0]) {
    appState.selectedShareApplicationId = filteredShareApplications.value[0].id;
  }
}

function selectShareTypeFilter(type) {
  appState.shareTypeFilter = type;
  if (!filteredShareApplications.value.some((item) => item.id === appState.selectedShareApplicationId) && filteredShareApplications.value[0]) {
    appState.selectedShareApplicationId = filteredShareApplications.value[0].id;
  }
  appState.shareListPage = 1;
}

function selectShareStatusFilter(status) {
  appState.shareStatusFilter = status;
  if (!filteredShareApplications.value.some((item) => item.id === appState.selectedShareApplicationId) && filteredShareApplications.value[0]) {
    appState.selectedShareApplicationId = filteredShareApplications.value[0].id;
  }
  appState.shareListPage = 1;
}

function resetServicesPage(scope) {
  if (scope === "access") {
    appState.accessPage = 1;
    return;
  }
  if (scope === "share-review") {
    appState.shareReviewPage = 1;
    return;
  }
  if (scope === "shared-data") {
    appState.sharedDataPage = 1;
    return;
  }
  if (scope === "share-calls") {
    appState.sharedCallPage = 1;
    return;
  }
  appState.shareListPage = 1;
}

function setServicesPageSize(size) {
  appState.servicesPageSize = Number(size);
  resetServicesPage(appState.shareModule === "access" ? "access" : appState.shareTertiary);
}

function changeServicesPage(direction) {
  const nextPage = Math.min(
    Math.max(1, currentServicesPage.value + direction),
    currentServicesPageCount.value,
  );

  if (appState.shareModule === "access") {
    appState.accessPage = nextPage;
    return;
  }
  if (appState.shareTertiary === "share-review") {
    appState.shareReviewPage = nextPage;
    return;
  }
  if (appState.shareTertiary === "shared-data") {
    appState.sharedDataPage = nextPage;
    return;
  }
  if (appState.shareTertiary === "share-calls") {
    appState.sharedCallPage = nextPage;
    return;
  }
  appState.shareListPage = nextPage;
}

function resetServicesFilters() {
  if (appState.shareModule === "access") {
    appState.accessTypeFilter = "全部";
    appState.accessStatusFilter = "全部";
    appState.accessKeyword = "";
    resetServicesPage("access");
    return;
  }

  if (appState.shareTertiary === "share-review") {
    appState.shareReviewStatusFilter = "全部";
    appState.shareReviewStepFilter = "全部";
    appState.shareReviewKeyword = "";
    resetServicesPage("share-review");
    return;
  }

  if (appState.shareTertiary === "shared-data") {
    appState.sharedDataMethodFilter = "全部";
    appState.sharedDataStatusFilter = "全部";
    appState.sharedDataKeyword = "";
    resetServicesPage("shared-data");
    return;
  }

  if (appState.shareTertiary === "share-calls") {
    appState.sharedCallMethodFilter = "全部";
    appState.sharedCallStatusFilter = "全部";
    appState.sharedCallKeyword = "";
    resetServicesPage("share-calls");
    return;
  }

  appState.shareTypeFilter = "全部";
  appState.shareStatusFilter = "全部";
  appState.shareKeyword = "";
  resetServicesPage("share-list");
}

function selectSystem(id) {
  appState.selectedSystemId = id;
}

function selectAsset(id) {
  appState.selectedAssetId = id;
}

function openAssetDetail(id) {
  appState.selectedAssetId = id;
  appState.view = "asset-detail";
}

function backToAssets() {
  setAssetModule(assetTypeModuleMap[selectedAsset.value?.type] ?? "asset-master");
  appState.view = "assets";
}

function selectMetric(id) {
  appState.selectedMetricId = id;
}

function openMetricDetail(id) {
  appState.selectedMetricId = id;
  setAssetModule("asset-indicators");
  appState.view = "metric-detail";
}

function backToMetrics() {
  setAssetModule("asset-indicators");
  appState.view = "assets";
}

function selectService(id) {
  appState.selectedServiceId = id;
}

function openServiceDetail(id) {
  appState.selectedServiceId = id;
  appState.view = "service-detail";
}

function applyShareRequestPreset(serviceId) {
  const service = serviceCatalog.find((item) => item.id === serviceId) ?? serviceCatalog[0];
  Object.assign(shareRequestForm, {
    ...shareRequestForm,
    ...buildShareRequestDraft(service),
    applyUnit: shareRequestForm.applyUnit,
    applicant: shareRequestForm.applicant,
    applyTime: shareRequestForm.applyTime,
  });
}

function openShareCreate(serviceId) {
  const service = serviceCatalog.find((item) => item.id === serviceId) ?? selectedService.value ?? serviceCatalog[0];
  Object.assign(shareRequestForm, buildShareRequestDraft(service));
  appState.view = "share-create";
  appState.shareModule = "share";
  appState.shareTertiary = "share-list";
}

function openShareDetail(id) {
  appState.selectedShareApplicationId = id;
  appState.view = "share-detail";
  appState.shareModule = "share";
  appState.shareTertiary = "share-list";
}

function backToShareList() {
  appState.view = "services";
  appState.shareModule = "share";
  appState.shareTertiary = "share-list";
}

function openShareReviewPage() {
  appState.view = "services";
  appState.shareModule = "share";
  appState.shareTertiary = "share-review";
}

function openShareReviewDetail(id) {
  appState.selectedShareApplicationId = id;
  appState.view = "share-review-detail";
  appState.shareModule = "share";
  appState.shareTertiary = "share-review";
}

function backToShareReview() {
  appState.view = "services";
  appState.shareModule = "share";
  appState.shareTertiary = "share-review";
}

function openSharedDataPage() {
  appState.view = "services";
  appState.shareModule = "share";
  appState.shareTertiary = "shared-data";
}

function openSharedDataDetail(id) {
  appState.selectedSharedDataId = id;
  appState.view = "shared-data-detail";
  appState.shareModule = "share";
  appState.shareTertiary = "shared-data";
}

function backToSharedDataList() {
  appState.view = "services";
  appState.shareModule = "share";
  appState.shareTertiary = "shared-data";
}

function openSharedDataFromApplication(requestId) {
  const target = sharedDataCatalog.value.find((item) => item.requestId === requestId);
  if (!target) {
    openSharedDataPage();
    return;
  }
  appState.selectedSharedDataId = target.id;
  appState.view = "shared-data-detail";
  appState.shareModule = "share";
  appState.shareTertiary = "shared-data";
}

function nextShareApplicationNo() {
  return `GZWL-GX-202607-${String(shareApplications.value.length + 1).padStart(3, "0")}`;
}

function buildSharedDataRecords(application) {
  const records = [];
  const methodList =
    application.shareMethod === "表下载 + 接口推送"
      ? ["表下载", "接口推送"]
      : [application.shareMethod];
  const baseIndex = sharedDataCatalog.value.length + 1;
  const tableStem =
    application.shareType === "旅游资源"
      ? "mdm_tourism_resource_share"
      : application.shareType === "主题数据集"
        ? "dws_tourism_topic_share"
        : "ads_tourism_metric_share";

  methodList.forEach((method, index) => {
    records.push({
      id: `shared-${baseIndex + index}`,
      requestId: application.id,
      requestNo: application.applicationNo,
      serviceId: application.serviceId,
      serviceName: application.serviceName,
      dataName: `${application.serviceName}${method === "接口推送" ? "接口数据" : "共享表"}`,
      tableName: `${tableStem}_${String(baseIndex + index).padStart(3, "0")}`,
      shareMethod: method,
      rowCount: `${(18 + baseIndex * 7 + index * 3).toLocaleString("zh-CN")}万行`,
      status: "已开通",
      openedTime: "2026-07-09 16:30",
      interfaceDetail: {
        endpoint: method === "接口推送" ? `/api/share/service/${application.serviceId}/v1/push` : "不适用",
        method: method === "接口推送" ? "POST" : "-",
        auth: method === "接口推送" ? "Token + IP 白名单" : "-",
        request: method === "接口推送" ? `按申请授权范围推送${application.serviceName}对应字段明细和结果值。` : "当前共享方式为表下载。",
        response: method === "接口推送" ? "返回共享批次编号、接收状态和失败重推信息。" : "下载后可直接落库或本地复核。",
      },
      downloadDetail:
        method === "表下载"
          ? `支持 CSV / XLSX 下载，字段范围与申请单 ${application.applicationNo} 审批结果保持一致。`
          : "当前共享方式为接口推送，不提供离线表下载。",
    });
  });

  return records;
}

function buildSharedDataCallRecords(records, application) {
  const baseIndex = sharedDataCallCatalog.value.length + 1;
  return records.map((record, index) => ({
    id: `call-${baseIndex + index}`,
    sharedDataId: record.id,
    requestNo: application.applicationNo,
    dataName: record.dataName,
    callerUnit: application.applyUnit,
    callStatus: "成功",
    callStartTime: "2026-07-09 17:00:00",
    callEndTime: "2026-07-09 17:00:24",
    callMethod: record.shareMethod === "接口推送" ? "API接口调用" : "批量下载",
    sharedVolume: record.rowCount,
    serviceName: application.serviceName,
  }));
}

function submitShareRequest() {
  const service = serviceCatalog.find((item) => item.id === shareRequestForm.serviceId) ?? serviceCatalog[0];
  const newApplication = {
    id: `share-${shareApplications.value.length + 1}`,
    applicationNo: nextShareApplicationNo(),
    serviceId: service.id,
    serviceName: service.name,
    shareType: shareRequestForm.shareType,
    dataDescription: shareRequestForm.dataDescription,
    scopeDescription: shareRequestForm.scopeDescription,
    shareMethod: shareRequestForm.shareMethod,
    shareFrequency: shareRequestForm.shareFrequency,
    secrecyLevel: shareRequestForm.secrecyLevel,
    ownerDept: shareRequestForm.ownerDept,
    applyUnit: shareRequestForm.applyUnit,
    applicant: shareRequestForm.applicant,
    applyTime: shareRequestForm.applyTime,
    status: "待责任单位确认",
    currentStep: "责任单位确认",
    currentReviewer: shareRequestForm.ownerDept,
    usageScene: shareRequestForm.scopeDescription,
    reviewTimeline: [
      { step: "提交申请", time: shareRequestForm.applyTime, status: "已完成", detail: `申请单位 ${shareRequestForm.applyUnit} 已提交共享申请。` },
      { step: "责任单位确认", time: "待处理", status: "进行中", detail: `等待 ${shareRequestForm.ownerDept} 确认共享范围、字段边界和责任归属。` },
      { step: "数据办审核", time: "未开始", status: "未开始", detail: "责任单位确认后进入数据办审核环节。" },
      { step: "共享开通", time: "未开始", status: "未开始", detail: "审核通过后生成共享清单并完成授权开通。" },
    ],
  };

  shareApplications.value.unshift(newApplication);
  appState.selectedShareApplicationId = newApplication.id;
  feedback.value = `${newApplication.applicationNo} 已提交，当前已进入审核流程。`;
  appState.view = "share-review-detail";
  appState.shareModule = "share";
}

function approveShareRequest(id = appState.selectedShareApplicationId) {
  const target = shareApplications.value.find((item) => item.id === id);
  if (!target) return;

  target.status = "已开通";
  target.currentStep = "共享已开通";
  target.currentReviewer = "信息中心";
  target.reviewTimeline = target.reviewTimeline.map((item) => {
    if (item.step === "责任单位确认" && item.status !== "已完成") {
      return { ...item, time: "2026-07-09 13:20", status: "已完成", detail: `${target.ownerDept} 已确认共享范围与责任边界。` };
    }
    if (item.step === "数据办审核") {
      return { ...item, time: "2026-07-09 15:10", status: "已完成", detail: "数据办已完成审核，允许开通共享服务。" };
    }
    if (item.step === "共享开通") {
      return { ...item, time: "2026-07-09 16:30", status: "已完成", detail: "已生成共享清单，接口和下载权限已开通。" };
    }
    return item;
  });

  if (!sharedDataCatalog.value.some((item) => item.requestId === target.id)) {
    const openedRecords = buildSharedDataRecords(target);
    sharedDataCatalog.value.unshift(...openedRecords);
    sharedDataCallCatalog.value.unshift(...buildSharedDataCallRecords(openedRecords, target));
  } else {
    sharedDataCatalog.value = sharedDataCatalog.value.map((item) =>
      item.requestId === target.id ? { ...item, status: "已开通", openedTime: "2026-07-09 16:30" } : item,
    );
  }

  const openedRecord = sharedDataCatalog.value.find((item) => item.requestId === target.id);
  if (openedRecord) {
    appState.selectedSharedDataId = openedRecord.id;
  }
  feedback.value = `${target.applicationNo} 已审核通过，并完成共享开通。`;
}

function selectAccessItem(id) {
  appState.selectedAccessId = id;
}

function openAccessDetail(id) {
  appState.selectedAccessId = id;
  appState.view = "access-detail";
}

function backToAccessList() {
  appState.view = "services";
  appState.shareModule = "access";
}

function backToServiceList() {
  appState.view = "services";
  appState.shareModule = "share";
}

function openSystemLogin(system) {
  if (!system?.loginUrl) return;
  window.open(system.loginUrl, "_blank", "noopener,noreferrer");
}

function selectPermissionModule(module) {
  appState.permissionModule = module;
}

function selectAnalysisModule(module) {
  appState.analysisModule = module;
}

function selectBusinessBoardModule(module) {
  appState.businessBoardModule = module;
}

function selectBusinessBoardView(view) {
  appState.businessBoardView = view;
}

function selectUser(id) {
  appState.selectedUserId = id;
}

function selectRole(id) {
  appState.selectedRoleId = id;
}

function selectOrg(id) {
  appState.selectedOrgId = id;
}

function showAnalysisTooltip(event, title, meta) {
  analysisTooltip.visible = true;
  analysisTooltip.x = event.clientX + 16;
  analysisTooltip.y = event.clientY + 16;
  analysisTooltip.title = title;
  analysisTooltip.meta = meta;
}

function hideAnalysisTooltip() {
  analysisTooltip.visible = false;
}

function toggleRoleFunction(module) {
  const role = selectedRole.value;
  if (!role) return;
  const index = role.functionModules.indexOf(module);
  if (index >= 0) {
    role.functionModules.splice(index, 1);
    return;
  }
  role.functionModules.push(module);
}

function toggleRoleDataPermission(permission) {
  const role = selectedRole.value;
  if (!role) return;
  const index = role.dataPermissions.indexOf(permission);
  if (index >= 0) {
    role.dataPermissions.splice(index, 1);
    return;
  }
  role.dataPermissions.push(permission);
}

function mapRegionFill(value) {
  const alpha = 0.26 + (value / analysisMapMax.value) * 0.6;
  return `rgba(15, 124, 99, ${alpha.toFixed(2)})`;
}

function ringSegments(items) {
  const total = items.reduce((sum, item) => sum + item.value, 0) || 1;
  const palette = ["#0f7c63", "#2d9f87", "#54b398", "#8ccaa6", "#b87922", "#d59a43"];
  let start = -90;
  return items.map((item, index) => {
    const sweep = (item.value / total) * 360;
    const end = start + sweep;
    const segment = {
      ...item,
      color: palette[index % palette.length],
      path: donutPath(78, 78, 60, 34, start, end),
      title: `${item.label}：${item.value} 项`,
    };
    start = end;
    return segment;
  });
}

function donutPath(cx, cy, outerR, innerR, startAngle, endAngle) {
  const startOuter = polarPoint(cx, cy, outerR, startAngle);
  const endOuter = polarPoint(cx, cy, outerR, endAngle);
  const startInner = polarPoint(cx, cy, innerR, endAngle);
  const endInner = polarPoint(cx, cy, innerR, startAngle);
  const largeArc = endAngle - startAngle > 180 ? 1 : 0;
  return [
    `M ${startOuter.x} ${startOuter.y}`,
    `A ${outerR} ${outerR} 0 ${largeArc} 1 ${endOuter.x} ${endOuter.y}`,
    `L ${startInner.x} ${startInner.y}`,
    `A ${innerR} ${innerR} 0 ${largeArc} 0 ${endInner.x} ${endInner.y}`,
    "Z",
  ].join(" ");
}

function polarPoint(cx, cy, r, angle) {
  const radians = (angle * Math.PI) / 180;
  return {
    x: +(cx + r * Math.cos(radians)).toFixed(2),
    y: +(cy + r * Math.sin(radians)).toFixed(2),
  };
}

function itemsByNames(items, names) {
  const nameSet = new Set(names);
  return items.filter((item) => nameSet.has(item.name));
}

const resourceRelationMap = {
  景区: {
    systems: ["旅游资源基础数据", "贵州全域旅游数字化服务平台", "黄小西智能体", "应急指挥子系统（在建）"],
    assets: ["旅游资源主数据", "景区客流主题数据集", "景区知识库", "统一指标口径库"],
    metrics: ["发布客流", "实时在园人数", "日累计入园人数", "活动风险等级"],
    services: ["旅游资源主数据查询 API", "景区客流专题数据集", "景区伴游知识服务", "旅游安全预警推送接口"],
  },
  街区: {
    systems: ["旅游资源基础数据", "活动管理系统", "贵州全域旅游数字化服务平台"],
    assets: ["旅游资源主数据", "活动传播评估主题集", "文旅消费热度主题集"],
    metrics: ["消费热度指数", "活动传播指数", "游客来源占比"],
    services: ["旅游资源主数据查询 API", "文旅消费热度服务", "活动传播评估接口"],
  },
  度假区: {
    systems: ["旅游资源基础数据", "贵州全域旅游数字化服务平台", "我行商旅"],
    assets: ["旅游资源主数据", "住宿经营运行主题集", "全域运行监测主题集"],
    metrics: ["住宿入住率", "平均房价", "消费热度指数"],
    services: ["旅游资源主数据查询 API", "住宿运行专题数据集", "文旅消费热度服务"],
  },
  乡村旅游点: {
    systems: ["贵州省乡村旅游综合管理平台", "旅游资源基础数据", "活动管理系统"],
    assets: ["旅游资源主数据", "游客画像标签库", "活动传播评估主题集"],
    metrics: ["游客来源占比", "消费热度指数", "活动传播指数"],
    services: ["旅游资源主数据查询 API", "活动传播评估接口", "文旅消费热度服务"],
  },
  文化场馆: {
    systems: ["贵州数字文化馆", "旅游资源基础数据"],
    assets: ["文化资源主数据", "数字文化资源知识库", "全域运行监测专题集"],
    metrics: ["游客来源占比", "消费热度指数"],
    services: ["旅游资源主数据查询 API", "智能问数与知识检索服务"],
  },
  酒店民宿: {
    systems: ["住业普查系统", "PMS（待接入数据）", "酒店智能体", "我行商旅"],
    assets: ["住宿资源主数据", "住宿经营运行主题集", "酒店知识库"],
    metrics: ["住宿入住率", "平均房价", "过夜游客量"],
    services: ["住宿运行专题数据集", "酒店知识库检索服务", "旅游资源主数据查询 API"],
  },
  旅行社: {
    systems: ["贵州省文化和旅游市场监督执法平台-旅游方向", "贵州省文旅信用监管平台"],
    assets: ["涉旅主体主数据", "文旅信用主题集", "涉旅主体信用标签库"],
    metrics: ["游客来源占比", "消费热度指数"],
    services: ["旅游资源主数据查询 API", "统一指标查询服务"],
  },
  导游: {
    systems: ["贵州省文旅信用监管平台", "贵州省文化和旅游市场监督执法平台-旅游方向"],
    assets: ["涉旅主体主数据", "涉旅主体信用标签库"],
    metrics: ["游客来源占比"],
    services: ["统一指标查询服务", "智能问数与知识检索服务"],
  },
  餐饮主体: {
    systems: ["贵州省文化和旅游市场监督执法平台-旅游方向", "贵州省文旅信用监管平台", "LBS与消费数据"],
    assets: ["涉旅主体主数据", "旅游市场监管主题集", "文旅消费热度主题集", "涉旅主体信用标签库"],
    metrics: ["消费热度指数", "游客来源占比"],
    services: ["文旅消费热度服务", "统一指标查询服务"],
  },
  购物主体: {
    systems: ["贵州省文化和旅游市场监督执法平台-旅游方向", "贵州省文旅信用监管平台", "LBS与消费数据"],
    assets: ["涉旅主体主数据", "旅游市场监管主题集", "文旅消费热度主题集", "涉旅主体信用标签库"],
    metrics: ["消费热度指数", "游客来源占比"],
    services: ["文旅消费热度服务", "统一指标查询服务"],
  },
  旅游服务主体: {
    systems: ["贵州省文化和旅游市场监督执法平台-旅游方向", "贵州省文旅信用监管平台", "LBS与消费数据"],
    assets: ["涉旅主体主数据", "旅游市场监管主题集", "文旅消费热度主题集", "涉旅主体信用标签库"],
    metrics: ["消费热度指数", "游客来源占比"],
    services: ["文旅消费热度服务", "统一指标查询服务"],
  },
};

function findMetricNamesByDomain(domains) {
  const domainSet = new Set(domains);
  return metrics.filter((item) => domainSet.has(item.domain)).map((item) => item.name);
}

function assetRelationBundle(asset) {
  switch (asset.type) {
    case "主数据资产":
      return {
        systems: ["旅游资源基础数据", "住业普查系统", "贵州省乡村旅游综合管理平台", "贵州省文化和旅游市场监督执法平台-旅游方向"],
        metrics: ["发布客流", "住宿入住率", "游客来源占比"],
        services: ["旅游资源主数据查询 API", "统一指标查询服务"],
      };
    case "主题数据资产":
      return {
        systems: ["贵州全域旅游数字化服务平台", "活动管理系统", "“行游贵州”旅游安全预警提示系统", "PMS（待接入数据）"],
        metrics: findMetricNamesByDomain(["客流监测", "住宿运行", "市场运行", "活动传播", "安全应急"]),
        services: ["景区客流专题数据集", "住宿运行专题数据集", "文旅消费热度服务", "活动传播评估接口", "旅游安全预警推送接口"],
      };
    case "指标资产":
      return {
        systems: ["贵州全域旅游数字化服务平台", "“行游贵州”旅游安全预警提示系统"],
        metrics: metrics.map((item) => item.name),
        services: ["统一指标查询服务", "智能问数服务"],
      };
    case "标签资产":
      return {
        systems: ["LBS与消费数据", "贵州省文旅信用监管平台", "贵客荟"],
        metrics: ["游客来源占比", "消费热度指数"],
        services: ["文旅消费热度服务", "智能问数服务"],
      };
    case "知识资产":
      return {
        systems: ["黄小西智能体", "酒店智能体", "贵州数字文化馆", "应急指挥子系统（在建）"],
        metrics: ["活动传播指数", "活动风险等级"],
        services: ["景区伴游知识服务", "酒店知识库检索服务", "智能问数与知识检索服务"],
      };
    case "服务资产":
      return {
        systems: ["贵州全域旅游数字化服务平台", "政府智能体", "应急指挥子系统（在建）"].filter((name) =>
          businessSystems.some((item) => item.name === name),
        ),
        metrics: ["发布客流", "住宿入住率", "活动风险等级", "消费热度指数"],
        services: [asset.name],
      };
    default:
      return { systems: [], metrics: [], services: [] };
  }
}

function serviceRelationBundle(service) {
  const name = service.name;
  if (name.includes("主数据")) {
    return {
      systems: ["旅游资源基础数据", "住业普查系统"],
      assets: ["旅游资源主数据", "住宿资源主数据", "涉旅主体主数据"],
      metrics: ["发布客流", "住宿入住率"],
    };
  }
  if (name.includes("客流")) {
    return {
      systems: ["贵州全域旅游数字化服务平台", "应急指挥子系统（在建）"],
      assets: ["景区客流主题数据集", "统一指标口径库"],
      metrics: ["发布客流", "实时在园人数", "日累计入园人数"],
    };
  }
  if (name.includes("住宿")) {
    return {
      systems: ["PMS（待接入数据）", "住业普查系统", "我行商旅"],
      assets: ["住宿经营运行主题集", "住宿资源主数据"],
      metrics: ["住宿入住率", "平均房价", "过夜游客量"],
    };
  }
  if (name.includes("传播")) {
    return {
      systems: ["活动管理系统"],
      assets: ["活动传播评估主题集"],
      metrics: ["活动传播指数"],
    };
  }
  if (name.includes("预警") || name.includes("视频")) {
    return {
      systems: ["“行游贵州”旅游安全预警提示系统", "应急指挥子系统（在建）"],
      assets: ["安全预警与事件处置主题集", "视频监控专题资源", "气象风险专题集"],
      metrics: ["活动风险等级", "视频在线率", "事件处置闭环率"],
    };
  }
  return {
    systems: ["黄小西智能体", "酒店智能体", "贵州全域旅游数字化服务平台"].filter((entry) =>
      businessSystems.some((item) => item.name === entry),
    ),
    assets: ["景区知识库", "酒店知识库", "统一指标口径库"].filter((entry) => dataAssets.some((item) => item.name === entry)),
    metrics: ["发布客流", "消费热度指数"],
  };
}

const selectedResourceRelations = computed(() => {
  const bundle = resourceRelationMap[selectedResource.value?.category] ?? { systems: [], assets: [], metrics: [], services: [] };
  return {
    systems: itemsByNames(businessSystems, bundle.systems),
    assets: itemsByNames(dataAssets, bundle.assets),
    metrics: itemsByNames(metrics, bundle.metrics),
    services: itemsByNames(serviceCatalog, bundle.services),
  };
});

const selectedSystemRelations = computed(() => {
  const system = selectedSystem.value;
  const assets = dataAssets.filter((item) => item.source.includes(system.name) || item.serviceScenes.includes(system.name));
  const services = serviceCatalog.filter((item) => item.detail.includes(system.name) || item.provider.includes(system.name) || item.name.includes(system.name));
  const metricsRelated = metrics.filter(
    (item) =>
      item.source.includes(system.name) ||
      (system.name.includes("PMS") && item.domain === "住宿运行") ||
      (system.name.includes("活动") && item.domain === "活动传播") ||
      (system.name.includes("应急") && item.domain === "安全应急"),
  );
  return { assets, services, metrics: metricsRelated };
});

const selectedAssetRelations = computed(() => {
  const asset = selectedAsset.value;
  const bundle = assetRelationBundle(asset);
  return {
    systems: itemsByNames(businessSystems, bundle.systems).filter((item) => asset.source.includes(item.name) || bundle.systems.includes(item.name)),
    metrics: itemsByNames(metrics, bundle.metrics),
    services: itemsByNames(serviceCatalog, bundle.services),
    resources: resources.filter((item) => {
      if (asset.name.includes("旅游资源主数据")) return ["景区", "街区", "度假区", "乡村旅游点"].includes(item.category);
      if (asset.name.includes("住宿")) return item.category === "酒店民宿";
      if (asset.name.includes("涉旅主体")) return ["旅行社", "导游", "餐饮主体", "购物主体", "旅游服务主体"].includes(item.category);
      if (asset.name.includes("文化")) return item.category === "文化场馆";
      if (asset.name.includes("活动")) return ["景区", "街区", "乡村旅游点"].includes(item.category);
      if (asset.name.includes("景区")) return item.category === "景区";
      return item.linkedData.some((entry) => asset.name.includes(entry) || entry.includes(asset.name.slice(0, 2)));
    }).slice(0, 8),
  };
});

const assetResourceJumpTargets = {
  "旅游资源主数据": [
    { label: "景区", category: "景区" },
    { label: "街区", category: "街区" },
    { label: "度假区", category: "度假区" },
    { label: "乡村旅游点", category: "乡村旅游点" },
  ],
  "住宿资源主数据": [{ label: "酒店民宿", category: "酒店民宿" }],
  "涉旅主体主数据": [
    { label: "旅行社", category: "旅行社" },
    { label: "导游", category: "导游" },
    { label: "餐饮主体", category: "餐饮主体" },
    { label: "购物主体", category: "购物主体" },
    { label: "旅游服务主体", category: "旅游服务主体" },
  ],
  "文化资源主数据": [{ label: "文化场馆", category: "文化场馆" }],
  "景区客流主题数据集": [{ label: "景区", category: "景区" }],
  "住宿经营运行主题集": [{ label: "酒店民宿", category: "酒店民宿" }],
  "活动传播评估主题集": [
    { label: "景区", category: "景区" },
    { label: "街区", category: "街区" },
    { label: "乡村旅游点", category: "乡村旅游点" },
  ],
  "全域运行监测主题集": [
    { label: "景区", category: "景区" },
    { label: "街区", category: "街区" },
    { label: "度假区", category: "度假区" },
    { label: "乡村旅游点", category: "乡村旅游点" },
    { label: "文化场馆", category: "文化场馆" },
    { label: "酒店民宿", category: "酒店民宿" },
  ],
};

const assetDomainMap = {
  "旅游资源主数据": "客流监测",
  "景区客流主题数据集": "客流监测",
  "住宿资源主数据": "住宿运行",
  "住宿经营运行主题集": "住宿运行",
  "涉旅主体主数据": "市场运行",
  "文化资源主数据": "市场运行",
  "活动传播评估主题集": "活动传播",
  "游客画像标签库": "游客画像",
  "安全预警与事件处置主题集": "安全应急",
  "视频监控专题资源": "安全应急",
  "气象风险专题集": "安全应急",
};

const domainAssetMap = {
  客流监测: "旅游资源主数据",
  住宿运行: "住宿资源主数据",
  市场运行: "涉旅主体主数据",
  游客画像: "游客画像标签库",
  活动传播: "活动传播评估主题集",
  安全应急: "安全预警与事件处置主题集",
};

const selectedAssetResourceTargets = computed(() => assetResourceJumpTargets[selectedAsset.value?.name] ?? []);

const selectedMetricRelations = computed(() => {
  const metric = selectedMetric.value;
  return {
    assets: dataAssets.filter((item) => item.serviceScenes.includes(metric.domain) || item.name.includes(metric.domain.slice(0, 2)) || metric.source.includes(item.name)).slice(0, 6),
    services: serviceCatalog.filter((item) => item.detail.includes(metric.name) || item.detail.includes(metric.domain) || item.name.includes(metric.domain.slice(0, 2))).slice(0, 6),
    systems: businessSystems.filter((item) => metric.source.includes(item.name) || item.purpose.includes(metric.domain) || item.content.includes(metric.domain)).slice(0, 6),
  };
});

const selectedServiceRelations = computed(() => {
  const bundle = serviceRelationBundle(selectedService.value);
  return {
    systems: itemsByNames(businessSystems, bundle.systems),
    assets: itemsByNames(dataAssets, bundle.assets),
    metrics: itemsByNames(metrics, bundle.metrics),
  };
});

const selectedShareRelations = computed(() => {
  const bundle = serviceRelationBundle(selectedShareSourceService.value);
  return {
    systems: itemsByNames(businessSystems, bundle.systems),
    assets: itemsByNames(dataAssets, bundle.assets),
    metrics: itemsByNames(metrics, bundle.metrics),
  };
});

const serviceRelatedApplications = computed(() =>
  shareApplications.value.filter((item) => item.serviceId === selectedService.value?.id),
);

function jumpToResource(resourceId) {
  const targetResource = resources.find((item) => item.id === resourceId);
  if (targetResource) {
    appState.resourceGroup = resourceGroupIdByCategory(targetResource.category);
    appState.resourceCategory = targetResource.category;
  }
  appState.selectedResourceId = resourceId;
  appState.view = "resource-detail";
}

function jumpToResourceCategory(category) {
  appState.resourceGroup = resourceGroupIdByCategory(category);
  appState.resourceCategory = category;
  const nextResource = resources.find((item) => item.category === category);
  if (nextResource) {
    appState.selectedResourceId = nextResource.id;
  }
  appState.view = "resources";
}

function jumpToSystem(systemId) {
  const targetSystem = businessSystems.find((item) => item.id === systemId);
  if (!targetSystem) return;
  if (targetSystem.type === "业务系统") {
    appState.systemType = "全部";
    appState.selectedSystemId = systemId;
    appState.view = "systems";
    return;
  }
  appState.view = "services";
  appState.shareModule = "access";
  const targetAccessItem = dataAccessCatalog.find((item) => item.originSystem === targetSystem.name);
  if (targetAccessItem) {
    appState.selectedAccessId = targetAccessItem.id;
    return;
  }
  if (dataAccessCatalog[0]) {
    appState.selectedAccessId = dataAccessCatalog[0].id;
  }
}

function jumpToAsset(assetId) {
  const asset = dataAssets.find((item) => item.id === assetId);
  setAssetModule(assetTypeModuleMap[asset?.type] ?? "asset-master");
  appState.selectedAssetId = assetId;
  appState.view = "asset-detail";
}

function jumpToMetric(metricId, domain) {
  if (domain) {
    appState.metricDomain = domain;
  }
  appState.selectedMetricId = metricId;
  setAssetModule("asset-indicators");
  appState.view = "metric-detail";
}

function jumpToMetricDomain(domain) {
  appState.metricDomain = domain;
  appState.metricQuery = "";
  const nextMetric = metrics.find((item) => item.domain === domain);
  if (nextMetric) {
    appState.selectedMetricId = nextMetric.id;
  }
  setAssetModule("asset-indicators");
  appState.view = "assets";
}

function jumpFromAssetToMetrics(asset) {
  const domain = assetDomainMap[asset?.name];
  appState.metricDomain = domain ?? "全部";
  appState.metricQuery = "";
  const nextMetric = domain ? metrics.find((item) => item.domain === domain) : metrics[0];
  if (nextMetric) {
    appState.selectedMetricId = nextMetric.id;
  }
  setAssetModule("asset-indicators");
  appState.view = "assets";
}

function jumpFromMetricDomainToAsset(domain) {
  const assetName = domainAssetMap[domain];
  const asset = dataAssets.find((item) => item.name === assetName);
  if (!asset) return;
  setAssetModule(assetTypeModuleMap[asset.type] ?? "asset-master");
  appState.selectedAssetId = asset.id;
  appState.view = "asset-detail";
}

function jumpToService(serviceId) {
  appState.shareModule = "share";
  appState.serviceType = "全部";
  appState.selectedServiceId = serviceId;
  appState.view = "service-detail";
}

function badgeTone(status) {
  if (status === "已开通" || status === "已接入" || status === "已沉淀" || status === "已运行" || status === "成功") return "primary";
  if (status === "审核中" || status === "补充材料" || status === "建设中" || status === "待接入" || status === "进行中" || status === "待开通") return "warn";
  return "neutral";
}
</script>

<template>
  <div v-if="!appState.isAuthenticated" class="login-shell">
    <section class="login-panel">
      <div class="login-orb login-orb-left"></div>
      <div class="login-orb login-orb-right"></div>
      <section class="surface login-card">
        <div class="login-card-brand">
          <div class="login-logo-mark">数</div>
          <p class="eyebrow">贵州省文化和旅游厅</p>
          <h1>文旅资源与数据资产门户</h1>
          <p class="login-desc">统一查看资源、业务系统、数据资产中心、接入共享和驾驶舱运行情况。</p>
        </div>

        <form class="request-form login-form" @submit.prevent="loginWithAccount()">
          <label>
            <span>登录账号</span>
            <select v-model="loginForm.account">
              <option v-for="item in loginAccountOptions" :key="item.id" :value="item.account">
                {{ item.name }} / {{ item.account }}
              </option>
            </select>
          </label>
          <label>
            <span>登录密码</span>
            <input v-model="loginForm.password" type="password" placeholder="请输入密码" />
          </label>
          <button type="submit" class="primary-button login-submit">登录</button>
          <p class="login-message">{{ loginMessage }}</p>
        </form>

        <div class="login-divider"><span>可用账户</span></div>

        <div class="login-account-inline-list">
          <button
            v-for="item in userDirectory"
            :key="`login-${item.id}`"
            class="login-account-inline"
            :disabled="item.status !== '启用'"
            @click="quickLogin(item.id)"
          >
            <strong>{{ item.name }}</strong>
            <span>{{ item.account }} / {{ item.password }}</span>
          </button>
        </div>

        <div class="login-footer-meta">
          <span>启用账户 {{ enabledUserCount }} 个</span>
          <span>在线用户 {{ onlineUserCount }} 个</span>
        </div>
      </section>
    </section>
  </div>

  <div v-else class="app-shell">
    <aside class="sidebar">
      <div class="brand">
        <div class="brand-mark">数</div>
        <div class="brand-copy">
          <p class="eyebrow">贵州省文化和旅游厅</p>
          <h1>文旅资源与数据资产门户</h1>
        </div>
      </div>

      <nav class="nav-list" aria-label="主导航">
        <div v-for="item in navigation" :key="item.id" class="nav-group">
          <button
            class="nav-item"
            :class="{ active: currentPrimaryNavId === item.id }"
            @click="setView(item.id)"
          >
            <component :is="item.icon" :size="18" />
            <span>{{ item.label }}</span>
          </button>

          <div v-if="currentPrimaryNavId === item.id && secondaryNavigation.length" class="nav-group-children">
            <div class="subnav-list">
              <button
                v-for="child in secondaryNavigation"
                :key="`${item.id}-${child.id}`"
                class="subnav-item"
                :class="{ active: activeSecondaryNavId === child.id }"
                @click="openSecondaryNav(child.id)"
              >
                <span>{{ child.label }}</span>
                <strong v-if="secondaryNavCount(child.id)">{{ secondaryNavCount(child.id) }}</strong>
              </button>
            </div>
          </div>
        </div>
      </nav>

    </aside>

    <main class="main">
      <header class="topbar">
        <div class="topbar-main">
          <div class="topbar-copy">
            <p class="topbar-breadcrumb">{{ pageBreadcrumb }}</p>
            <div class="topbar-title-row">
              <h2>{{ selectedNav.label }}</h2>
              <span v-if="selectedNav.tag" class="header-tag">{{ selectedNav.tag }}</span>
            </div>
          </div>
          <div class="topbar-userbox">
            <button class="topbar-user-trigger" @click="appState.view = 'permissions'; appState.permissionModule = 'users'">
              <div class="topbar-user-avatar">{{ currentUserInitial }}</div>
              <div class="topbar-user-meta">
                <strong>{{ currentUser?.name }}</strong>
                <div class="topbar-user-tags">
                  <span class="user-role-tag">{{ currentUser?.role }}</span>
                  <span class="user-state-dot" :class="currentUser?.loginState === '在线' ? 'online' : 'offline'"></span>
                </div>
              </div>
              <ChevronDown :size="16" />
            </button>
            <button class="topbar-text-action" @click="signOut">退出</button>
          </div>
        </div>
      </header>

      <section v-if="appState.view === 'dashboard'" class="view active">
        <section class="surface dashboard-welcome">
          <div class="dashboard-welcome-copy">
            <h3>{{ welcomeGreeting }}，{{ currentUser?.name }}</h3>
            <p>{{ currentUserAreaLabel }} · {{ currentUser?.role }}</p>
          </div>
          <div class="dashboard-welcome-date">{{ portalDateText }}</div>
        </section>

        <div class="dashboard-message-strip">
          <button
            v-for="item in dashboardMessageCards"
            :key="item.id"
            type="button"
            class="surface dashboard-message-card"
            :class="item.tone"
            @click="setView(item.id)"
          >
            <div class="dashboard-message-icon">
              <component :is="item.icon" :size="18" />
            </div>
            <div class="dashboard-message-main">
              <div class="dashboard-message-head">
                <div>
                  <p class="eyebrow">{{ item.eyebrow }}</p>
                  <div class="dashboard-message-title-row">
                    <h3>{{ item.title }}</h3>
                    <span class="dashboard-message-priority" :class="item.tone">{{ item.priority }}</span>
                  </div>
                </div>
                <span class="dashboard-message-count">{{ item.count }}</span>
              </div>
              <div class="dashboard-message-body">
                <div class="dashboard-message-summary">{{ item.summary }}</div>
                <strong>{{ item.leadTitle }}</strong>
                <p>{{ item.leadMeta }}</p>
                <span>{{ item.detail }}</span>
              </div>
            </div>
            <div class="dashboard-message-action">
              <span>{{ item.actionText }}</span>
              <ChevronRight :size="16" />
            </div>
          </button>
        </div>

        <div class="stats-grid">
          <article v-for="card in dashboardCards" :key="card.label" class="stat-card">
            <h3>{{ card.label }}</h3>
            <div class="stat-value">{{ card.value }}</div>
            <div v-if="card.meta" class="stat-meta">
              <span>{{ card.meta }}</span>
            </div>
          </article>
        </div>

        <div class="dashboard-grid">
          <section class="surface panel-span-8">
            <div class="section-head">
              <div>
                <p class="eyebrow">全省态势</p>
                <h3>全省运行趋势</h3>
              </div>
              <span class="caption">游客总量、过夜游客量、旅游收入按月趋势</span>
            </div>
            <div class="bar-chart dashboard-trend-chart">
              <article
                v-for="item in dashboardOperationTrend"
                :key="item.month"
                class="bar-item"
                @mousemove="showAnalysisTooltip($event, item.month, `游客：${item.visitors}万人次 / 过夜：${item.stay}万人 / 收入：${item.revenue}亿元`)"
                @mouseleave="hideAnalysisTooltip"
              >
                <div class="bar-track">
                  <div class="bar-fill access" :style="{ height: `${(item.visitors / dashboardTrendMax) * 100}%` }"></div>
                </div>
                <strong>{{ item.visitors }}</strong>
                <span>{{ item.month }}</span>
              </article>
            </div>
            <div class="dashboard-trend-meta">
              <article class="mini-stat">
                <span>7月游客总量</span>
                <strong>128.6万人次</strong>
              </article>
              <article class="mini-stat">
                <span>7月过夜游客</span>
                <strong>32.8万人</strong>
              </article>
              <article class="mini-stat">
                <span>7月旅游收入</span>
                <strong>6.7亿元</strong>
              </article>
            </div>
          </section>

          <section class="surface panel-span-4">
            <div class="section-head">
              <div>
                <p class="eyebrow">空间分布</p>
                <h3>贵州资源分布</h3>
              </div>
            </div>
            <div class="dashboard-map-shell">
              <div class="guizhou-map-shell compact">
                <svg class="guizhou-map" viewBox="0 0 620 500" aria-label="贵州省资源地图">
                  <path class="province-outline" :d="guizhouProvincePath" />
                  <g v-for="region in analysisResourceCounts" :key="region.id">
                    <path
                      class="region-shape"
                      :d="region.path"
                      :fill="mapRegionFill(region.value)"
                      stroke="#d8e1dc"
                      stroke-width="2"
                      @mousemove="showAnalysisTooltip($event, region.name, `资源总量：${region.value}`)"
                      @mouseleave="hideAnalysisTooltip"
                    />
                    <text :x="region.labelX" :y="region.labelY" text-anchor="middle">{{ region.name }}</text>
                    <text :x="region.labelX" :y="region.labelY + 18" text-anchor="middle" class="map-value">{{ region.value }}</text>
                  </g>
                </svg>
              </div>
            </div>
          </section>

          <section v-for="item in dashboardTopicCards" :key="item.title" class="surface panel-span-4 dashboard-topic-card">
            <div class="dashboard-topic-head">
              <div class="dashboard-topic-heading">
                <p class="eyebrow topic-eyebrow">{{ item.eyebrow }}</p>
                <h3>{{ item.title }}</h3>
              </div>
              <button class="action-link topic-action" @click="setView(item.action)">查看专题</button>
            </div>
            <div class="dashboard-topic-layout">
              <article
                v-for="(metric, index) in item.metrics"
                :key="metric.label"
                class="dashboard-topic-metric"
                :class="{ primary: index === 0 }"
              >
                <span>{{ metric.label }}</span>
                <strong>{{ metric.value }}</strong>
                <p v-if="index === 0">当前专题重点监测指标</p>
              </article>
            </div>
            <div class="dashboard-topic-note">
              <span>运行提示</span>
              <p>{{ item.note }}</p>
            </div>
          </section>

          <section v-for="item in dashboardSecurityCards" :key="item.title" class="surface panel-span-4">
            <div class="section-head">
              <div>
                <p class="eyebrow">{{ item.eyebrow }}</p>
                <h3>{{ item.title }}</h3>
              </div>
            </div>
            <div class="dashboard-metric-grid">
              <article v-for="metric in item.metrics" :key="metric.label" class="dashboard-metric-card">
                <span>{{ metric.label }}</span>
                <strong>{{ metric.value }}</strong>
              </article>
            </div>
            <p class="dashboard-card-note">{{ item.note }}</p>
          </section>

          <section v-for="item in dashboardFoundationCards" :key="item.title" class="surface panel-span-6">
            <div class="section-head">
              <div>
                <p class="eyebrow">{{ item.eyebrow }}</p>
                <h3>{{ item.title }}</h3>
              </div>
            </div>
            <div class="dashboard-metric-grid metrics-4">
              <article v-for="metric in item.metrics" :key="metric.label" class="dashboard-metric-card">
                <span>{{ metric.label }}</span>
                <strong>{{ metric.value }}</strong>
              </article>
            </div>
            <p class="dashboard-card-note">{{ item.note }}</p>
          </section>
        </div>
      </section>

      <section v-if="appState.view === 'dashboard-todos'" class="view active">
        <section class="surface">
          <div class="section-head">
            <div>
              <p class="eyebrow">驾驶舱消息</p>
              <h3>待办清单</h3>
            </div>
            <span class="caption">共 {{ dashboardTodoItems.length }} 项</span>
          </div>
          <div class="ops-table">
            <div class="ops-table-head">
              <span>事项名称</span>
              <span>责任单位</span>
              <span>办理时限</span>
              <span>操作</span>
            </div>
            <article v-for="item in dashboardTodoItems" :key="item.title" class="ops-row" @click="openDashboardLink(item)">
              <div class="ops-main">
                <div class="ops-main-top">
                  <strong>{{ item.title }}</strong>
                  <span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span>
                </div>
                <p>{{ item.detail }}</p>
                <p>{{ item.category }} · 协同 {{ item.coord }}</p>
              </div>
              <div class="ops-owner">
                <span class="caption">责任单位</span>
                <strong>{{ item.unit }}</strong>
              </div>
              <div class="ops-owner">
                <span class="caption">办理时限</span>
                <strong>{{ item.deadline }}</strong>
              </div>
              <div class="ops-action">
                <button class="action-link" @click.stop="openDashboardLink(item)">{{ item.actionLabel }}</button>
              </div>
            </article>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'dashboard-warnings'" class="view active">
        <section class="surface">
          <div class="section-head">
            <div>
              <p class="eyebrow">驾驶舱消息</p>
              <h3>预警提示</h3>
            </div>
            <span class="caption">共 {{ dashboardWarningItems.length }} 项</span>
          </div>
          <div class="ops-table">
            <div class="ops-table-head">
              <span>预警事项</span>
              <span>责任单位</span>
              <span>触发方式</span>
              <span>操作</span>
            </div>
            <article v-for="item in dashboardWarningItems" :key="item.title" class="ops-row" @click="openDashboardLink(item)">
              <div class="ops-main">
                <div class="ops-main-top">
                  <strong>{{ item.title }}</strong>
                  <span class="badge" :class="item.status === '关注' ? 'neutral' : 'warn'">{{ item.status }}</span>
                </div>
                <p>{{ item.detail }}</p>
                <p>{{ item.category }} · 协同 {{ item.coord }}</p>
              </div>
              <div class="ops-owner">
                <span class="caption">责任单位</span>
                <strong>{{ item.unit }}</strong>
              </div>
              <div class="ops-owner">
                <span class="caption">触发方式</span>
                <strong>{{ item.trigger }}</strong>
              </div>
              <div class="ops-action">
                <button class="action-link" @click.stop="openDashboardLink(item)">{{ item.actionLabel }}</button>
              </div>
            </article>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'dashboard-monitor'" class="view active">
        <div class="content-grid">
          <section class="surface span-8">
            <div class="section-head">
              <div>
                <p class="eyebrow">运行监控</p>
                <h3>贵州资源分布</h3>
              </div>
              <label class="inline-select">
                <span>统计口径</span>
                <select v-model="appState.analysisMetric">
                  <option v-for="item in analysisMetricOptions" :key="item.id" :value="item.id">{{ item.label }}</option>
                </select>
              </label>
            </div>
            <div class="map-analytics">
              <div class="guizhou-map-shell">
                <svg class="guizhou-map" viewBox="0 0 620 500" aria-label="贵州省资源地图">
                  <path class="province-outline" :d="guizhouProvincePath" />
                  <g v-for="region in analysisResourceCounts" :key="`monitor-${region.id}`">
                    <path
                      class="region-shape"
                      :d="region.path"
                      :fill="mapRegionFill(region.value)"
                      stroke="#d8e1dc"
                      stroke-width="2"
                      @mousemove="showAnalysisTooltip($event, region.name, `${region.dimension}：${region.value}`)"
                      @mouseleave="hideAnalysisTooltip"
                    />
                    <text :x="region.labelX" :y="region.labelY" text-anchor="middle">{{ region.name }}</text>
                    <text :x="region.labelX" :y="region.labelY + 18" text-anchor="middle" class="map-value">{{ region.value }}</text>
                  </g>
                </svg>
              </div>
            </div>
          </section>

          <section class="surface span-4">
            <div class="section-head">
              <div>
                <p class="eyebrow">地图说明</p>
                <h3>地州资源排行</h3>
              </div>
            </div>
            <div class="analysis-side-panel">
              <article class="map-description-card">
                <strong>展示说明</strong>
                <p>按贵州九个市州展示资源分布，颜色越深表示资源数量越高。</p>
              </article>
              <article
                v-for="region in analysisRankedRegions.slice(0, 6)"
                :key="`monitor-rank-${region.id}`"
                class="rank-item"
                @mousemove="showAnalysisTooltip($event, region.name, `${region.dimension}：${region.value}`)"
                @mouseleave="hideAnalysisTooltip"
              >
                <div>
                  <strong>{{ region.name }}</strong>
                  <p>{{ region.dimension }}</p>
                </div>
                <span>{{ region.value }}</span>
              </article>
            </div>
          </section>

          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">数据资产监控</p>
                <h3>数据资产目录分类汇总</h3>
              </div>
              <span class="caption">按数据资产目录和指标目录归集统计</span>
            </div>
            <div class="stats-grid monitor-asset-stat-grid">
              <article v-for="item in dashboardAssetMonitorCards" :key="item.label" class="stat-card asset-stat-card">
                <h3>{{ item.label }}</h3>
                <div class="stat-value">{{ item.value }}</div>
                <div class="stat-meta"><span>{{ item.meta }}</span></div>
              </article>
            </div>
            <div class="table-wrap compact-table-wrap monitor-table-wrap">
              <table class="data-table monitor-asset-table">
                <colgroup>
                  <col style="width: 15%" />
                  <col style="width: 12%" />
                  <col style="width: 20%" />
                  <col style="width: 13%" />
                  <col style="width: 14%" />
                  <col style="width: 13%" />
                  <col style="width: 13%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>资产分类</th>
                    <th>目录数量</th>
                    <th>分类说明</th>
                    <th>责任单位</th>
                    <th>更新频率</th>
                    <th>共享状态</th>
                    <th>占比</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in dashboardAssetTypeRows" :key="item.type">
                    <td><strong>{{ item.type }}</strong></td>
                    <td>{{ item.count }} 项</td>
                    <td>{{ item.description }}</td>
                    <td>{{ item.owner }}</td>
                    <td>{{ item.refresh }}</td>
                    <td>
                      <span class="badge" :class="item.buildingCount ? 'warn' : 'primary'">
                        {{ item.readyCount }} 已就绪 / {{ item.buildingCount }} 建设中
                      </span>
                    </td>
                    <td>
                      <div class="asset-ratio-cell">
                        <i :style="{ width: `${item.ratio}%` }"></i>
                        <em>{{ item.ratio }}%</em>
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>

          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">数据接入</p>
                <h3>采集任务运行监控</h3>
              </div>
              <span class="caption">累计执行、近24小时状态与今日任务明细</span>
            </div>
            <div class="stats-grid stats-inline-4 monitor-stat-grid">
              <article v-for="item in dashboardMonitorTaskCards" :key="item.label" class="stat-card">
                <h3>{{ item.label }}</h3>
                <div class="stat-value">{{ item.value }}</div>
                <div class="stat-meta"><span>{{ item.meta }}</span></div>
              </article>
            </div>
            <div class="table-wrap compact-table-wrap monitor-table-wrap">
              <table class="data-table monitor-task-table">
                <colgroup>
                  <col style="width: 28%" />
                  <col style="width: 12%" />
                  <col style="width: 18%" />
                  <col style="width: 18%" />
                  <col style="width: 12%" />
                  <col style="width: 12%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>任务名称</th>
                    <th>任务状态</th>
                    <th>开始时间</th>
                    <th>结束时间</th>
                    <th>持续时间</th>
                    <th>监控口径</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in dashboardTodayTaskDetails" :key="item.taskName">
                    <td><strong>{{ item.taskName }}</strong></td>
                    <td><span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span></td>
                    <td>{{ item.startTime }}</td>
                    <td>{{ item.endTime }}</td>
                    <td>{{ item.duration }}</td>
                    <td>今日任务</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>

          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">数据共享</p>
                <h3>服务调用运行监控</h3>
              </div>
              <span class="caption">服务规模、累计调用、状态分布与今日调用明细</span>
            </div>
            <div class="monitor-share-layout">
              <div class="stats-grid stats-inline-4 monitor-stat-grid">
                <article v-for="item in dashboardShareMonitorCards" :key="item.label" class="stat-card">
                  <h3>{{ item.label }}</h3>
                  <div class="stat-value">{{ item.value }}</div>
                  <div class="stat-meta"><span>{{ item.meta }}</span></div>
                </article>
              </div>
              <article class="monitor-donut-panel">
                <div class="monitor-donut-gauge" :style="{ '--rate': `${dashboardSharedCallSuccessRate}%` }">
                  <div>
                    <strong>{{ dashboardSharedCallSuccessRate }}%</strong>
                    <span>成功率</span>
                  </div>
                </div>
                <div class="monitor-donut-legend">
                  <span><i class="success"></i>成功 {{ dashboardSharedCallSuccess }} 次</span>
                  <span><i class="failed"></i>失败 {{ dashboardSharedCallFailed }} 次</span>
                </div>
              </article>
            </div>
            <div class="table-wrap compact-table-wrap monitor-table-wrap">
              <table class="data-table monitor-call-table">
                <colgroup>
                  <col style="width: 26%" />
                  <col style="width: 19%" />
                  <col style="width: 16%" />
                  <col style="width: 17%" />
                  <col style="width: 10%" />
                  <col style="width: 12%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>服务名称</th>
                    <th>调用者信息</th>
                    <th>调用时间</th>
                    <th>共享清单</th>
                    <th>调用状态</th>
                    <th>调用方式</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in sharedDataCallCatalog" :key="`monitor-${item.id}`">
                    <td><strong>{{ item.serviceName }}</strong></td>
                    <td>{{ item.callerUnit }}</td>
                    <td>{{ item.callStartTime }}</td>
                    <td>{{ item.dataName }}</td>
                    <td><span class="badge" :class="badgeTone(item.callStatus)">{{ item.callStatus }}</span></td>
                    <td>{{ item.callMethod }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
        </div>
      </section>

      <section v-if="appState.view === 'resources'" class="view active">
        <div class="section-toolbar">
          <div class="toolbar-context">
            <span class="toolbar-label">三级菜单</span>
            <strong>{{ activeResourceGroup.label }}</strong>
          </div>
          <div class="filter-chips">
            <button
              v-for="category in resourceCategories"
              :key="`${appState.resourceGroup}-${category}`"
              class="chip"
              :class="{ active: appState.resourceCategory === category }"
              @click="selectCategory(category)"
            >
              {{ category }}
            </button>
          </div>
        </div>

        <section class="surface">
          <div class="section-head">
            <div>
              <p class="eyebrow">资源清单</p>
              <h3>单体资源管理</h3>
            </div>
            <span class="caption">共 {{ filteredResources.length }} 条</span>
          </div>
          <div v-if="filteredResources.length" class="table-wrap">
            <table class="data-table resource-table">
              <thead>
                <tr>
                  <th>资源名称</th>
                  <th>资源类型</th>
                  <th>{{ resourceLevelColumnLabel }}</th>
                  <th v-if="showResourceStatusColumn">营业状态</th>
                  <th>市州 / 区县</th>
                  <th v-if="showResourceChargeColumn">收费情况</th>
                  <th v-if="showResourceOpeningColumn">开放时间</th>
                  <th>关联数据</th>
                  <th>操作</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="item in filteredResources"
                  :key="item.id"
                  class="clickable-row"
                  :class="{ selected: selectedResource?.id === item.id }"
                >
                  <td>
                    <strong>{{ item.name }}</strong>
                    <p class="caption table-subtext">{{ item.address }}</p>
                  </td>
                  <td>{{ item.category }}</td>
                  <td>{{ item.level }}</td>
                  <td v-if="showResourceStatusColumn">{{ resourceOperationStatus(item) }}</td>
                  <td>{{ item.city }} / {{ item.county }}</td>
                  <td v-if="showResourceChargeColumn">{{ item.chargeMode }}</td>
                  <td v-if="showResourceOpeningColumn">{{ item.openingHours }}</td>
                  <td>{{ item.linkedData.length }} 项</td>
                  <td>
                    <button class="action-link" @click="openResourceDetail(item.id)">查看</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div v-else class="empty-state">
            <MapPinned :size="20" />
            <strong>未找到匹配资源</strong>
            <p>请调整搜索关键词或切换资源分类后重试。</p>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'resource-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToResources">
                <ChevronLeft :size="16" />
                返回资源管理
              </button>
              <p class="eyebrow">资源详情</p>
              <h3>{{ selectedResource?.name }}</h3>
            </div>
            <span class="caption">已整理 {{ selectedResourceFieldCount }} 个基础字段</span>
          </div>
          <div v-if="selectedResource" class="detail-content">
            <div class="detail-summary">
              <span class="badge primary">{{ selectedResource.category }}</span>
              <span class="pill">{{ selectedResource.level }}</span>
              <span class="pill">{{ selectedResource.city }} / {{ selectedResource.county }}</span>
              <span class="pill">{{ selectedResource.linkedData.length }} 项关联数据</span>
            </div>

            <div v-for="group in selectedResourceSections" :key="group.title" class="detail-block">
              <div class="detail-block-head">
                <h4>{{ group.title }}</h4>
                <span v-if="group.note" class="caption">{{ group.note }}</span>
              </div>
              <div class="detail-grid">
                <div
                  v-for="entry in group.fields"
                  :key="`${group.title}-${entry.label}`"
                  class="detail-item"
                  :class="{ 'full-span': isFullSpanField(entry) }"
                >
                  <span>{{ entry.label }}</span>
                  <div v-if="Array.isArray(entry.value)" class="detail-list">
                    <span v-for="item in entry.value" :key="item" class="pill">{{ item }}</span>
                  </div>
                  <strong v-else class="detail-value">{{ entry.value }}</strong>
                </div>
              </div>
            </div>

            <div class="detail-block">
              <h4>已关联数据</h4>
              <div class="detail-list">
                <span v-for="entry in selectedResource.linkedData" :key="entry" class="pill">{{ entry }}</span>
              </div>
            </div>

            <div class="detail-block">
              <h4>关联链路</h4>
              <div class="link-block">
                <span>业务系统</span>
                <div class="jump-list">
                  <button
                    v-for="item in selectedResourceRelations.systems"
                    :key="item.id"
                    class="jump-pill"
                    @click="jumpToSystem(item.id)"
                  >
                    {{ item.name }}
                  </button>
                </div>
              </div>
              <div class="link-block">
                <span>数据资产</span>
                <div class="jump-list">
                  <button
                    v-for="item in selectedResourceRelations.assets"
                    :key="item.id"
                    class="jump-pill"
                    @click="jumpToAsset(item.id)"
                  >
                    {{ item.name }}
                  </button>
                </div>
              </div>
              <div class="link-block">
                <span>指标资产</span>
                <div class="jump-list">
                  <button
                    v-for="item in selectedResourceRelations.metrics"
                    :key="item.id"
                    class="jump-pill"
                    @click="jumpToMetric(item.id)"
                  >
                    {{ item.name }}
                  </button>
                </div>
              </div>
              <div class="link-block">
                <span>数据共享</span>
                <div class="jump-list">
                  <button
                    v-for="item in selectedResourceRelations.services"
                    :key="item.id"
                    class="jump-pill"
                    @click="jumpToService(item.id)"
                  >
                    {{ item.name }}
                  </button>
                </div>
              </div>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'systems'" class="view active">
        <section class="surface">
          <div class="section-head">
            <div>
              <p class="eyebrow">系统清单</p>
              <h3>业务系统目录</h3>
            </div>
            <span class="caption">共 {{ filteredSystems.length }} 项</span>
          </div>
          <div class="system-grid">
            <button
              v-for="item in filteredSystems"
              :key="item.id"
              class="system-card"
              :class="{
                active: selectedSystem?.id === item.id,
                disabled: !item.loginUrl,
              }"
              :disabled="!item.loginUrl"
              @click="selectSystem(item.id); openSystemLogin(item)"
            >
              <div class="system-card-head">
                <span class="badge primary">业务系统</span>
                <span class="badge" :class="item.loginUrl ? 'primary' : 'neutral'">{{ item.loginUrl ? "进入" : "暂不可跳转" }}</span>
              </div>
              <h4>{{ item.name }}</h4>
              <div class="system-meta">
                <span>责任处室</span>
                <strong>{{ item.ownerDept }}</strong>
              </div>
            </button>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'assets'" class="view active">
        <template v-if="appState.assetModule === 'asset-overview'">
          <div class="stats-grid asset-overview-stats">
            <article v-for="card in assetCenterOverviewCards" :key="card.title" class="stat-card asset-stat-card">
              <h3>{{ card.title }}</h3>
              <div class="stat-value">{{ card.value }}</div>
              <div class="stat-meta">
                <span>{{ card.meta }}</span>
              </div>
            </article>
          </div>

          <div class="content-grid">
            <section class="surface span-7">
              <div class="section-head">
                <div>
                  <p class="eyebrow">资产分布</p>
                  <h3>按资产类型统计</h3>
                </div>
                <span class="caption">统一纳管数据资产与指标资产</span>
              </div>
              <div class="asset-overview-table">
                <div class="asset-overview-row head">
                  <span>资产类型</span>
                  <span>资产说明</span>
                  <span>占比</span>
                  <span>数量</span>
                </div>
                <button
                  v-for="item in assetTypeOverviewRows"
                  :key="item.title"
                  class="asset-overview-row"
                  @click="openAssetTypeOverview(item.title)"
                >
                  <strong>{{ item.title }}</strong>
                  <span>{{ item.description }}</span>
                  <div class="asset-ratio-cell">
                    <i :style="{ width: `${Math.max(item.ratio, 4)}%` }"></i>
                    <em>{{ item.ratio }}%</em>
                  </div>
                  <b>{{ item.count }} 项</b>
                </button>
              </div>
            </section>

            <section class="surface span-5">
              <div class="section-head">
                <div>
                  <p class="eyebrow">主题域</p>
                  <h3>指标与资产关联</h3>
                </div>
                <button class="action-link" @click="setAssetModule('asset-indicators')">查看指标资产</button>
              </div>
              <div class="table-wrap compact-table-wrap">
                <table class="data-table asset-domain-table">
                  <thead>
                    <tr>
                      <th>主题域</th>
                      <th>指标</th>
                      <th>关联资产</th>
                      <th>责任单位</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in assetThemeDomainRows.slice(0, 7)" :key="item.domain">
                      <td>
                        <button class="action-link" @click="jumpToMetricDomain(item.domain)">{{ item.domain }}</button>
                      </td>
                      <td>{{ item.metricCount }} 项</td>
                      <td>{{ item.assetCount }} 项</td>
                      <td>{{ item.owner }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </section>

          </div>
        </template>

        <div v-else-if="isAssetDirectoryModule" class="content-grid">
          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">{{ currentAssetDirectoryEyebrow }}</p>
                <h3>{{ currentAssetDirectoryTitle }}</h3>
              </div>
              <span class="caption">共 {{ filteredAssets.length }} 项</span>
            </div>
            <div class="table-wrap">
              <table class="data-table assets-table">
                <thead>
                  <tr>
                    <th>资产名称</th>
                    <th>资产类型</th>
                    <th>数据来源</th>
                    <th>更新频率</th>
                    <th>应用场景</th>
                    <th>状态</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in filteredAssets" :key="item.id">
                    <td><strong>{{ item.name }}</strong></td>
                    <td>{{ item.type }}</td>
                    <td>{{ item.source }}</td>
                    <td>{{ item.refresh }}</td>
                    <td>{{ item.serviceScenes }}</td>
                    <td><span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span></td>
                    <td>
                      <button class="action-link" @click="openAssetDetail(item.id)">查看</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
        </div>

        <template v-else-if="appState.assetModule === 'asset-indicators'">
          <div class="section-toolbar">
            <div class="toolbar-context">
              <span class="toolbar-label">查询条件</span>
              <strong>指标资产检索</strong>
            </div>
            <div class="inline-form-grid">
              <label class="inline-select">
                <span>指标名称</span>
                <input v-model="appState.metricQuery" type="search" placeholder="请输入指标名称" />
              </label>
              <label class="inline-select">
                <span>主题域</span>
                <select :value="appState.metricDomain" @change="selectMetricDomain($event.target.value)">
                  <option v-for="item in metricDomainOptions" :key="item" :value="item">{{ item }}</option>
                </select>
              </label>
            </div>
          </div>
          <section class="surface">
            <div class="section-head">
              <div>
                <p class="eyebrow">指标资产</p>
                <h3>指标口径与应用场景</h3>
              </div>
              <span class="caption">共 {{ tableMetrics.length }} 项</span>
            </div>
            <div class="table-wrap">
              <table class="data-table metrics-table">
                <thead>
                  <tr>
                    <th>指标名称</th>
                    <th>主题域</th>
                    <th>指标说明</th>
                    <th>口径说明</th>
                    <th>数据来源</th>
                    <th>更新频率</th>
                    <th>责任单位</th>
                    <th>应用场景</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in tableMetrics" :key="item.id">
                    <td><strong>{{ item.name }}</strong></td>
                    <td>
                      <button class="jump-pill inline-link" @click="jumpToMetricDomain(item.domain)">{{ item.domain }}</button>
                    </td>
                    <td>{{ item.description }}</td>
                    <td>{{ item.rule }}</td>
                    <td>{{ item.source }}</td>
                    <td>{{ item.refresh }}</td>
                    <td>{{ item.owner }}</td>
                    <td>{{ item.scenes }}</td>
                    <td>
                      <button class="action-link" @click="openMetricDetail(item.id)">查看</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
        </template>

      </section>

      <section v-if="appState.view === 'asset-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToAssets">
                <ChevronLeft :size="16" />
                返回资产目录
              </button>
              <p class="eyebrow">资产详情</p>
              <h3>{{ selectedAsset?.name }}</h3>
            </div>
          </div>
          <div v-if="selectedAsset" class="detail-content">
            <div class="detail-summary">
              <span class="badge primary">{{ selectedAsset.type }}</span>
              <span class="pill">{{ selectedAsset.status }}</span>
              <span class="pill">{{ selectedAsset.shareStatus }}</span>
            </div>
            <div class="detail-grid">
              <div class="detail-item"><span>责任人</span><strong>{{ selectedAsset.steward }}</strong></div>
              <div class="detail-item"><span>开放级别</span><strong>{{ selectedAsset.openLevel }}</strong></div>
              <div class="detail-item"><span>共享状态</span><strong>{{ selectedAsset.shareStatus }}</strong></div>
              <div class="detail-item"><span>更新频率</span><strong>{{ selectedAsset.refresh }}</strong></div>
              <div class="detail-item"><span>服务方式</span><strong>{{ selectedAsset.serviceMode }}</strong></div>
              <div class="detail-item full-span"><span>数据来源</span><strong class="detail-value">{{ selectedAsset.source }}</strong></div>
              <div class="detail-item full-span"><span>应用场景</span><strong class="detail-value">{{ selectedAsset.serviceScenes }}</strong></div>
              <div class="detail-item full-span"><span>质量规则</span><strong class="detail-value">{{ selectedAsset.qualityRule }}</strong></div>
            </div>
            <div class="detail-block">
              <h4>资产说明</h4>
              <p>{{ selectedAsset.assetSummary }}</p>
            </div>
            <div class="detail-block">
              <div class="detail-block-head">
                <h4>旅游资产主体中包括哪些内容</h4>
                <button
                  v-if="assetDomainMap[selectedAsset.name]"
                  class="action-link"
                  @click="jumpFromAssetToMetrics(selectedAsset)"
                >
                  查看关联指标
                </button>
              </div>
              <div v-if="selectedAssetResourceTargets.length" class="jump-list">
                <button
                  v-for="item in selectedAssetResourceTargets"
                  :key="`${selectedAsset.id}-${item.category}`"
                  class="jump-pill"
                  @click="jumpToResourceCategory(item.category)"
                >
                  {{ item.label }}
                </button>
              </div>
            </div>
            <div class="detail-block">
              <h4>核心字段</h4>
              <div class="detail-list">
                <span v-for="item in selectedAsset.assetFields" :key="item" class="pill">{{ item }}</span>
              </div>
            </div>
            <div class="detail-block">
              <h4>典型输出</h4>
              <div class="detail-list">
                <span v-for="item in selectedAsset.assetOutputs" :key="item" class="pill">{{ item }}</span>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联业务系统</h4>
              <div class="jump-list">
                <button v-for="item in selectedAssetRelations.systems" :key="item.id" class="jump-pill" @click="jumpToSystem(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联资源对象</h4>
              <div class="jump-list">
                <button v-for="item in selectedAssetRelations.resources" :key="item.id" class="jump-pill" @click="jumpToResource(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联指标</h4>
              <div class="jump-list">
                <button v-for="item in selectedAssetRelations.metrics" :key="item.id" class="jump-pill" @click="jumpToMetric(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联服务</h4>
              <div class="jump-list">
                <button v-for="item in selectedAssetRelations.services" :key="item.id" class="jump-pill" @click="jumpToService(item.id)">{{ item.name }}</button>
              </div>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'metric-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToMetrics">
                <ChevronLeft :size="16" />
                返回指标资产
              </button>
              <p class="eyebrow">指标资产详情</p>
              <h3>{{ selectedMetric?.name }}</h3>
            </div>
          </div>
          <div v-if="selectedMetric" class="detail-content">
            <div class="detail-summary">
              <button class="jump-pill domain-link" @click="jumpFromMetricDomainToAsset(selectedMetric.domain)">
                {{ selectedMetric.domain }}
              </button>
              <span class="pill">{{ selectedMetric.refresh }}</span>
              <span class="pill">{{ selectedMetric.owner }}</span>
            </div>
            <div class="detail-grid">
              <div class="detail-item full-span"><span>指标说明</span><strong class="detail-value">{{ selectedMetric.description }}</strong></div>
              <div class="detail-item full-span"><span>口径说明</span><strong class="detail-value">{{ selectedMetric.rule }}</strong></div>
              <div class="detail-item full-span"><span>数据来源</span><strong class="detail-value">{{ selectedMetric.source }}</strong></div>
              <div class="detail-item full-span"><span>应用场景</span><strong class="detail-value">{{ selectedMetric.scenes }}</strong></div>
            </div>
            <div class="detail-block">
              <div class="detail-block-head">
                <h4>关联数据资产</h4>
              <button class="action-link" @click="jumpFromMetricDomainToAsset(selectedMetric.domain)">按主题域查看资产详情</button>
              </div>
              <div class="jump-list">
                <button
                  v-for="item in selectedMetricRelations.assets"
                  :key="item.id"
                  class="jump-pill"
                  @click="jumpToMetricDomain(item.name === '统一指标口径库' ? selectedMetric.domain : (assetDomainMap[item.name] ?? selectedMetric.domain))"
                >
                  {{ item.name }}
                </button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联业务系统</h4>
              <div class="jump-list">
                <button v-for="item in selectedMetricRelations.systems" :key="item.id" class="jump-pill" @click="jumpToSystem(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联服务</h4>
              <div class="jump-list">
                <button v-for="item in selectedMetricRelations.services" :key="item.id" class="jump-pill" @click="jumpToService(item.id)">{{ item.name }}</button>
              </div>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'query'" class="view active">
        <div class="section-toolbar">
          <div class="toolbar-context">
            <span class="toolbar-label">三级菜单</span>
            <strong>{{ currentQueryModuleLabel }}</strong>
          </div>
          <div v-if="['query-explore', 'query-area', 'query-scenic'].includes(appState.queryModule)" class="toolbar-actions">
            <button class="secondary-button" @click="saveCurrentQuery">
              <Star :size="16" />
              保存查询
            </button>
            <button class="secondary-button" @click="exportCurrentQuery">
              <Download :size="16" />
              导出结果
            </button>
          </div>
        </div>

        <template v-if="appState.queryModule === 'query-overview'">
          <div class="stats-grid">
            <article v-for="card in queryOverviewCards" :key="card.title" class="stat-card">
              <h3>{{ card.title }}</h3>
              <div class="stat-value">{{ card.value }}</div>
              <div class="stat-meta">
                <span>{{ card.meta }}</span>
              </div>
            </article>
          </div>

          <div class="content-grid">
            <section class="surface span-8">
              <div class="section-head">
                <div>
                  <p class="eyebrow">查询主题</p>
                  <h3>按主题选择关键指标</h3>
                </div>
                <span class="caption">覆盖资源、客流、运营、住宿、消费、应急和底座运行</span>
              </div>
              <div class="query-theme-grid">
                <button
                  v-for="item in queryThemeCards"
                  :key="item.id"
                  class="query-theme-card"
                  :class="{ active: appState.queryTheme === item.id }"
                  @click="selectQueryTheme(item.id); selectQueryModule('query-explore')"
                >
                  <div class="query-theme-head">
                    <div>
                      <h4>{{ item.label }}</h4>
                      <p>{{ item.description }}</p>
                    </div>
                    <span class="badge primary">{{ item.metricCount }} 项</span>
                  </div>
                  <div class="query-theme-tags">
                    <span v-for="metric in item.hotMetrics" :key="`${item.id}-${metric}`" class="pill">{{ metric }}</span>
                  </div>
                </button>
              </div>
            </section>

            <section class="surface span-4">
              <div class="section-head">
                <div>
                  <p class="eyebrow">常用入口</p>
                  <h3>高频查询</h3>
                </div>
              </div>
              <div class="query-side-stack">
                <article class="query-side-card">
                  <div class="signal-meta">
                    <strong>快速开始</strong>
                    <span class="badge primary">自助查询</span>
                  </div>
                  <p>支持查询全省资源明细、7 月每日 LBS 客流、全年客流趋势、住宿运行和消费热度等关键数据。</p>
                  <button class="action-link" @click="selectQueryModule('query-explore')">进入多维查询</button>
                </article>
                <article class="query-side-card">
                  <div class="signal-meta">
                    <strong>专题报表</strong>
                    <span class="badge neutral">{{ queryReportTemplates.length }} 套</span>
                  </div>
                  <p>可直接调用处室常用报表模板，减少重复配置，提高专题研判效率。</p>
                  <button class="action-link" @click="selectQueryModule('query-reports')">查看报表模板</button>
                </article>
                <article class="query-side-card">
                  <div class="signal-meta">
                    <strong>我的查询</strong>
                    <span class="badge neutral">{{ savedQueryItems.length }} 项</span>
                  </div>
                  <p>已保存高频查询条件，便于领导、处室和市州单位直接复用同一口径。</p>
                  <button class="action-link" @click="selectQueryModule('query-favorites')">查看我的查询</button>
                </article>
              </div>
            </section>

            <section class="surface span-12">
              <div class="section-head">
                <div>
                  <p class="eyebrow">热门指标</p>
                  <h3>系统关键指标</h3>
                </div>
                <span class="caption">点击指标可直接进入对应查询</span>
              </div>
              <div class="query-metric-grid">
                <button
                  v-for="item in queryOverviewHotMetrics"
                  :key="item.id"
                  class="query-metric-card"
                  @click="selectQueryTheme(item.theme); selectQueryMetric(item.id); selectQueryModule('query-explore')"
                >
                  <div class="query-metric-card-top">
                    <strong>{{ item.label }}</strong>
                    <span class="pill">{{ item.unit }}</span>
                  </div>
                  <p>{{ item.description }}</p>
                  <span class="query-metric-card-meta">{{ queryThemeCatalog.find((theme) => theme.id === item.theme)?.label }}</span>
                </button>
              </div>
            </section>
          </div>
        </template>

        <template v-else-if="['query-explore', 'query-area', 'query-scenic'].includes(appState.queryModule)">
          <section class="surface query-filter-panel">
            <template v-if="appState.queryModule === 'query-explore'">
              <div class="section-head query-panel-head">
                <div>
                  <p class="eyebrow">查询条件</p>
                  <h3>多维查询配置</h3>
                </div>
                <div class="query-panel-actions">
                  <button class="secondary-button" @click="resetQueryFilters">重置条件</button>
                  <button class="primary-button" @click="executeQuery()">
                    <Search :size="16" />
                    查询
                  </button>
                  <button class="secondary-button" @click="saveCurrentQuery">
                    <Star :size="16" />
                    保存为常用查询
                  </button>
                </div>
              </div>
              <div class="query-form-grid query-form-grid-4">
                <label class="query-field">
                  <span>主题域</span>
                  <select :value="appState.queryTheme" @change="selectQueryTheme($event.target.value)">
                    <option v-for="item in queryThemeCatalog" :key="item.id" :value="item.id">{{ item.label }}</option>
                  </select>
                </label>
                <label class="query-field">
                  <span>关键指标</span>
                  <select :value="appState.queryMetricId" @change="selectQueryMetric($event.target.value)">
                    <option v-for="item in queryMetricOptions" :key="item.id" :value="item.id">{{ item.label }}</option>
                  </select>
                </label>
                <label v-if="showQueryTimeFields" class="query-field">
                  <span>统计周期</span>
                  <select :value="appState.queryTimePreset" @change="selectQueryTimePreset($event.target.value)">
                    <option v-for="item in queryTimePresetOptions" :key="item.id" :value="item.id">{{ item.label }}</option>
                  </select>
                </label>
                <label v-if="showQueryTimeFields" class="query-field">
                  <span>时间粒度</span>
                  <select :value="appState.queryGranularity" @change="selectQueryGranularity($event.target.value)">
                    <option v-for="item in queryGranularityOptions" :key="item" :value="item">{{ item }}</option>
                  </select>
                </label>
                <label v-if="showQueryRegionField" class="query-field">
                  <span>统计范围</span>
                  <select v-model="appState.queryRegion">
                    <option value="全省">全省</option>
                    <option v-for="item in queryCityOptions" :key="item" :value="item">{{ item }}</option>
                  </select>
                </label>
                <label class="query-field">
                  <span>分析维度</span>
                  <select v-model="appState.queryDimension">
                    <option v-for="item in queryDimensionOptions" :key="item" :value="item">{{ item }}</option>
                  </select>
                </label>
                <label v-if="showQuerySourceField" class="query-field">
                  <span>数据来源</span>
                  <select v-model="appState.querySource">
                    <option v-for="item in querySourceOptions" :key="item" :value="item">{{ item }}</option>
                  </select>
                </label>
                <label v-if="showQueryResourceFields" class="query-field">
                  <span>资源类型</span>
                  <select v-model="appState.queryResourceCategory">
                    <option v-for="item in queryResourceCategoryOptions" :key="item" :value="item">{{ item }}</option>
                  </select>
                </label>
                <label v-if="showQueryResourceFields" class="query-field">
                  <span>等级筛选</span>
                  <select v-model="appState.queryLevelFilter">
                    <option v-for="item in queryLevelOptions" :key="item" :value="item">{{ item }}</option>
                  </select>
                </label>
                <label v-if="showQueryResourceFields" class="query-field">
                  <span>收费情况</span>
                  <select v-model="appState.queryChargeFilter">
                    <option v-for="item in queryChargeOptions" :key="item" :value="item">{{ item }}</option>
                  </select>
                </label>
              </div>
            </template>

            <template v-else>
              <div class="section-head query-panel-head">
                <div>
                  <p class="eyebrow">{{ activeDedicatedQueryMeta.eyebrow }}</p>
                  <h3>{{ activeDedicatedQueryMeta.title }}</h3>
                </div>
                <div class="query-panel-actions">
                  <button class="secondary-button" @click="applyQueryConfigToDraft(activeDedicatedQueryMeta.defaultConfig); executeQuery('已恢复当前模块默认查询条件。')">重置条件</button>
                  <button class="primary-button" @click="executeQuery()">
                    <Search :size="16" />
                    查询
                  </button>
                  <button class="secondary-button" @click="exportCurrentQuery">
                    <Download :size="16" />
                    导出结果
                  </button>
                  <button class="secondary-button" @click="saveCurrentQuery">
                    <Star :size="16" />
                    保存查询
                  </button>
                </div>
              </div>
              <div class="query-dedicated-layout">
                <div class="query-form-grid query-form-grid-4">
                  <label class="query-field">
                    <span>查询类型</span>
                    <select :value="appState.queryMetricId" @change="selectQueryMetric($event.target.value)">
                      <option v-for="item in dedicatedQueryMetricOptions" :key="item.id" :value="item.id">{{ item.label }}</option>
                    </select>
                  </label>
                  <label class="query-field">
                    <span>统计周期</span>
                    <select :value="appState.queryTimePreset" @change="selectQueryTimePreset($event.target.value)">
                      <option v-for="item in queryTimePresetOptions" :key="item.id" :value="item.id">{{ item.label }}</option>
                    </select>
                  </label>
                  <label class="query-field">
                    <span>时间粒度</span>
                    <select :value="appState.queryGranularity" @change="selectQueryGranularity($event.target.value)">
                      <option v-for="item in queryGranularityOptions" :key="item" :value="item">{{ item }}</option>
                    </select>
                  </label>
                  <label class="query-field">
                    <span>{{ activeDedicatedQueryMeta.scopeLabel }}</span>
                    <select v-model="appState.queryRegion">
                      <option v-for="item in activeDedicatedQueryMeta.scopeOptions" :key="item" :value="item">{{ item }}</option>
                    </select>
                  </label>
                  <label v-if="appState.queryModule === 'query-area'" class="query-field">
                    <span>统计层级</span>
                    <select v-model="appState.queryDimension">
                      <option v-for="item in queryAreaLevelOptions" :key="item" :value="item === '省' ? '区域层级' : item">{{ item }}</option>
                    </select>
                  </label>
                  <label class="query-field">
                    <span>分析维度</span>
                    <select v-model="appState.queryDimension">
                      <option v-for="item in dedicatedQueryDimensionOptions" :key="item" :value="item">{{ item }}</option>
                    </select>
                  </label>
                  <label class="query-field">
                    <span>数据来源</span>
                    <select v-model="appState.querySource">
                      <option v-for="item in querySourceOptions" :key="item" :value="item">{{ item }}</option>
                    </select>
                  </label>
                </div>
                <div class="query-support-grid">
                  <article v-for="item in dedicatedQueryMetricOptions" :key="`support-${item.id}`" class="query-support-card" :class="{ active: appState.queryMetricId === item.id }">
                    <div class="query-metric-card-top">
                      <strong>{{ item.label }}</strong>
                      <span class="pill">{{ item.unit }}</span>
                    </div>
                    <p>{{ item.definition }}</p>
                    <div class="query-theme-tags">
                      <span v-for="dimension in item.dimensions.slice(0, 5)" :key="`${item.id}-${dimension}`" class="pill">{{ dimension }}</span>
                    </div>
                  </article>
                </div>
              </div>
            </template>
          </section>

          <div class="stats-grid">
            <article v-for="card in currentQuerySummaryCards" :key="card.title" class="stat-card">
              <h3>{{ card.title }}</h3>
              <div class="stat-value">{{ card.value }}</div>
              <div class="stat-meta">
                <span>{{ card.meta }}</span>
              </div>
            </article>
          </div>

          <div class="content-grid">
            <section class="surface span-9">
              <div class="section-head">
                <div>
                  <p class="eyebrow">结果分析</p>
                  <h3>{{ executedQueryMetric.label }}</h3>
                </div>
                <div class="toolbar-actions">
                  <div class="share-tertiary-tabs" role="tablist" aria-label="查询结果视图">
                    <button
                      v-for="item in queryResultViews"
                      :key="item.id"
                      class="share-tab"
                      :class="{ active: appState.queryResultView === item.id }"
                      @click="selectQueryResultView(item.id)"
                    >
                      <component :is="item.icon" :size="14" />
                      {{ item.label }}
                    </button>
                  </div>
                </div>
              </div>

              <div class="query-result-toolbar">
                <span class="caption">统计周期：{{ currentQueryTimeLabel }}</span>
                <span class="caption">统计范围：{{ appState.queryExecuted.region }}</span>
                <span class="caption">展示方式：{{ currentQueryResultViewLabel }}</span>
              </div>

              <div v-if="appState.queryResultView === 'trend'" class="query-result-block">
                <div class="query-line-chart">
                  <svg
                    class="query-line-chart-svg"
                    :viewBox="`0 0 ${currentQueryTrendChart.width} ${currentQueryTrendChart.height}`"
                    role="img"
                    :aria-label="`${executedQueryMetric.label}趋势折线图`"
                    preserveAspectRatio="none"
                  >
                    <defs>
                      <linearGradient id="queryTrendAreaFill" x1="0" x2="0" y1="0" y2="1">
                        <stop offset="0%" stop-color="#11785f" stop-opacity="0.24" />
                        <stop offset="100%" stop-color="#11785f" stop-opacity="0.02" />
                      </linearGradient>
                    </defs>
                    <g v-for="line in currentQueryTrendChart.gridLines" :key="`grid-${line.ratio}`" class="query-line-grid">
                      <line x1="42" :x2="currentQueryTrendChart.width - 42" :y1="line.y" :y2="line.y"></line>
                    </g>
                    <path
                      v-if="currentQueryTrendChart.areaPath"
                      class="query-line-area"
                      :d="currentQueryTrendChart.areaPath"
                      fill="url(#queryTrendAreaFill)"
                    ></path>
                    <path
                      v-if="currentQueryTrendChart.linePath"
                      class="query-line-path"
                      :d="currentQueryTrendChart.linePath"
                      fill="none"
                    ></path>
                    <g v-for="point in currentQueryTrendChart.chartPoints" :key="point.label">
                      <circle
                        class="query-line-point"
                        :cx="point.x"
                        :cy="point.y"
                        r="5"
                        @mousemove="showAnalysisTooltip($event, point.label, `${executedQueryMetric.label}：${point.displayValue}`)"
                        @mouseleave="hideAnalysisTooltip"
                      ></circle>
                      <text
                        v-if="point.showLabel"
                        class="query-line-value"
                        :x="point.x"
                        :y="Math.max(point.y - 12, 18)"
                        text-anchor="middle"
                      >
                        {{ point.displayValue }}
                      </text>
                      <text
                        v-if="point.showLabel"
                        class="query-line-axis-label"
                        :x="point.x"
                        :y="currentQueryTrendChart.chartBottom + 22"
                        text-anchor="middle"
                      >
                        {{ point.label }}
                      </text>
                    </g>
                  </svg>
                </div>
              </div>

              <div v-else-if="appState.queryResultView === 'distribution'" class="query-result-block query-rank-list">
                <article
                  v-for="item in currentQueryDistributionRows"
                  :key="item.label"
                  class="query-rank-item"
                  @mousemove="showAnalysisTooltip($event, item.label, `${executedQueryMetric.label}：${item.displayValue}`)"
                  @mouseleave="hideAnalysisTooltip"
                >
                  <div class="query-rank-main">
                    <div class="query-rank-head">
                      <strong>{{ item.label }}</strong>
                      <span>{{ item.displayValue }}</span>
                    </div>
                    <div class="query-rank-track">
                      <span
                        class="query-rank-fill"
                        :style="{ width: `${(item.value / Math.max(...currentQueryDistributionRows.map((row) => row.value), 1)) * 100}%` }"
                      ></span>
                    </div>
                  </div>
                  <span class="badge neutral">{{ item.ratio }}</span>
                </article>
              </div>

              <div v-else-if="appState.queryResultView === 'pivot'" class="query-result-block">
                <div class="query-result-toolbar">
                  <span class="caption">透视口径：{{ currentQueryMatrixCaption }}</span>
                  <span class="caption">统计方式：资源数量交叉统计</span>
                </div>
                <div class="table-wrap">
                  <table class="data-table query-pivot-table">
                    <thead>
                      <tr>
                        <th>{{ currentQueryMatrix.rowHeader || '维度' }}</th>
                        <th v-for="column in currentQueryMatrix.columns" :key="`pivot-column-${column}`">{{ column }}</th>
                        <th>合计</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="row in currentQueryMatrix.rows" :key="row.label">
                        <td><strong>{{ row.label }}</strong></td>
                        <td v-for="(cell, index) in row.cells" :key="`${row.label}-${index}`">{{ cell }}</td>
                        <td><strong>{{ row.total }}</strong></td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>

              <div v-else class="query-result-block">
                <div class="table-wrap">
                  <table class="data-table query-detail-table">
                    <thead>
                      <tr>
                        <th v-for="column in currentQueryColumns" :key="`detail-column-${column.key}`">{{ column.label }}</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="(row, rowIndex) in currentQueryRows" :key="`detail-row-${rowIndex}`">
                        <td v-for="column in currentQueryColumns" :key="`${rowIndex}-${column.key}`">
                          <button
                            v-if="column.key === 'actionLabel' && row._resourceId"
                            class="action-link"
                            @click="openResourceFromQuery(row._resourceId)"
                          >
                            {{ row[column.key] }}
                          </button>
                          <strong v-else-if="column.key === 'name'">{{ row[column.key] }}</strong>
                          <span v-else>{{ row[column.key] }}</span>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </section>

            <section class="surface span-3">
              <div class="section-head">
                <div>
                  <p class="eyebrow">关联能力</p>
                  <h3>结果联动</h3>
                </div>
              </div>
              <div class="query-side-stack">
                <article class="query-side-card">
                  <div class="signal-meta">
                    <strong>指标口径</strong>
                    <span class="badge primary">{{ executedQueryTheme.label }}</span>
                  </div>
                  <p>{{ executedQueryMetric.description }}</p>
                  <button class="action-link" @click="openQueryMetricDirectory">查看指标资产</button>
                </article>
                <article class="query-side-card">
                  <div class="signal-meta">
                    <strong>关联数据资产</strong>
                    <span class="badge neutral">{{ currentQueryMetricAssets.length }} 项</span>
                  </div>
                  <div class="jump-list">
                    <button
                      v-for="item in currentQueryMetricAssets"
                      :key="item.id"
                      class="jump-pill"
                      @click="jumpToAsset(item.id)"
                    >
                      {{ item.name }}
                    </button>
                  </div>
                </article>
                <article class="query-side-card">
                  <div class="signal-meta">
                    <strong>结果用途</strong>
                    <span class="badge neutral">{{ isResourceQuery ? '资源明细' : '趋势结果' }}</span>
                  </div>
                  <p v-if="isResourceQuery">适用于查看全省景区、酒店民宿、文化场馆和涉旅主体等统一底账明细。</p>
                  <p v-else>适用于查看月度趋势、日度波动、区域分布、多维透视和结果导出。</p>
                </article>
              </div>
            </section>
          </div>
        </template>

        <template v-else-if="appState.queryModule === 'query-reports'">
          <section class="surface">
            <div class="section-head">
              <div>
                <p class="eyebrow">专题报表</p>
                <h3>常用查询模板</h3>
              </div>
              <span class="caption">选择模板后自动带入指标、时间、区域和维度条件</span>
            </div>
            <div class="query-report-grid">
              <article v-for="item in queryReportTemplates" :key="item.id" class="query-report-card">
                <div class="query-report-head">
                  <div>
                    <h4>{{ item.title }}</h4>
                    <p>{{ item.summary }}</p>
                  </div>
                  <span class="badge primary">{{ queryThemeCatalog.find((theme) => theme.id === item.theme)?.label }}</span>
                </div>
                <div class="query-report-meta">
                  <span class="pill">指标：{{ queryMetricCatalog.find((metric) => metric.id === item.metricId)?.label }}</span>
                  <span class="pill">时间：{{ item.timePreset === 'month-2026-07' ? '2026年7月' : item.timePreset === 'year-2026' ? '2026年' : '近7日' }}</span>
                  <span class="pill">范围：{{ item.region }}</span>
                  <span class="pill">维度：{{ item.dimension }}</span>
                  <span v-if="item.queryModule" class="pill">入口：{{ item.queryModule === 'query-area' ? '区域查询详情' : '景区查询详情' }}</span>
                </div>
                <div class="toolbar-actions query-report-actions">
                  <button class="action-link" @click="applyQueryReportTemplate(item.id)">{{ item.queryModule ? '进入查询详情' : '载入模板' }}</button>
                </div>
              </article>
            </div>
          </section>
        </template>

        <template v-else>
          <section class="surface">
            <div class="section-head">
              <div>
                <p class="eyebrow">常用查询</p>
                <h3>我的查询</h3>
              </div>
              <span class="caption">共 {{ savedQueryItems.length }} 项</span>
            </div>
            <div class="table-wrap">
              <table class="data-table query-favorites-table">
                <thead>
                  <tr>
                    <th>查询名称</th>
                    <th>主题域</th>
                    <th>关键指标</th>
                    <th>统计范围</th>
                    <th>时间范围</th>
                    <th>更新时间</th>
                    <th>说明</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in savedQueryItems" :key="item.id">
                    <td><strong>{{ item.name }}</strong></td>
                    <td>{{ queryThemeCatalog.find((theme) => theme.id === item.theme)?.label }}</td>
                    <td>{{ queryMetricCatalog.find((metric) => metric.id === item.metricId)?.label }}</td>
                    <td>{{ item.region }}</td>
                    <td>{{ item.granularity }} / {{ item.timePreset === 'month-2026-07' ? '2026年7月' : item.timePreset === 'year-2026' ? '2026年' : '近7日' }}</td>
                    <td>{{ item.updatedAt }}</td>
                    <td>{{ item.summary }}</td>
                    <td>
                      <div class="table-action-stack">
                        <button class="action-link" @click="runSavedQuery(item.id)">使用</button>
                        <button class="action-link" @click="removeSavedQuery(item.id)">删除</button>
                      </div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
        </template>
      </section>

      <section v-if="appState.view === 'services'" class="view active">
        <div class="section-toolbar services-toolbar">
          <div class="toolbar-context">
            <span class="toolbar-label">三级菜单</span>
            <strong>{{ appState.shareModule === "access" ? "数据接入" : shareTertiaryLabel }}</strong>
          </div>
          <div v-if="appState.shareModule === 'share'" class="toolbar-actions share-toolbar-actions">
            <div class="share-tertiary-tabs" role="tablist" aria-label="数据共享三级菜单">
              <button
                v-for="item in shareTertiaryItems"
                :key="item.id"
                class="share-tab"
                :class="{ active: activeShareTertiaryId === item.id }"
                :aria-selected="activeShareTertiaryId === item.id"
                @click="openShareTertiaryNav(item.id)"
              >
                {{ item.label }}
              </button>
            </div>
          </div>
        </div>

        <section class="surface services-query-panel">
          <div class="section-head query-panel-head">
            <div>
              <p class="eyebrow">查询条件</p>
              <h3>筛选与检索</h3>
            </div>
            <div class="query-panel-actions">
              <button class="secondary-button" @click="resetServicesFilters">重置条件</button>
              <span class="caption">共 {{ currentServicesTotal }} 条记录</span>
            </div>
          </div>

          <div v-if="appState.shareModule === 'access'" class="query-form-grid">
            <label class="query-field">
              <span>数据类型</span>
              <select v-model="appState.accessTypeFilter" @change="resetServicesPage('access')">
                <option v-for="item in accessTypeOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field">
              <span>接入状态</span>
              <select v-model="appState.accessStatusFilter" @change="resetServicesPage('access')">
                <option v-for="item in accessStatusOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field query-field-wide">
              <span>关键字</span>
              <input v-model="appState.accessKeyword" placeholder="请输入数据名称、来源渠道或来源系统" @input="resetServicesPage('access')" />
            </label>
          </div>

          <div v-else-if="activeShareTertiaryId === 'share-list'" class="query-form-grid">
            <label class="query-field">
              <span>共享类型</span>
              <select v-model="appState.shareTypeFilter" @change="selectShareTypeFilter(appState.shareTypeFilter)">
                <option v-for="item in shareTypeOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field">
              <span>申请状态</span>
              <select v-model="appState.shareStatusFilter" @change="selectShareStatusFilter(appState.shareStatusFilter)">
                <option v-for="item in shareStatusOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field query-field-wide">
              <span>关键字</span>
              <input v-model="appState.shareKeyword" placeholder="请输入申请单号、服务名称、申请单位或申请人" @input="resetServicesPage('share-list')" />
            </label>
          </div>

          <div v-else-if="activeShareTertiaryId === 'share-review'" class="query-form-grid">
            <label class="query-field">
              <span>审核状态</span>
              <select v-model="appState.shareReviewStatusFilter" @change="resetServicesPage('share-review')">
                <option v-for="item in shareStatusOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field">
              <span>当前环节</span>
              <select v-model="appState.shareReviewStepFilter" @change="resetServicesPage('share-review')">
                <option v-for="item in shareReviewStepOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field query-field-wide">
              <span>关键字</span>
              <input v-model="appState.shareReviewKeyword" placeholder="请输入申请单号、服务名称、申请单位或处理人" @input="resetServicesPage('share-review')" />
            </label>
          </div>

          <div v-else-if="activeShareTertiaryId === 'shared-data'" class="query-form-grid">
            <label class="query-field">
              <span>共享方式</span>
              <select v-model="appState.sharedDataMethodFilter" @change="resetServicesPage('shared-data')">
                <option v-for="item in sharedDataMethodOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field">
              <span>开通状态</span>
              <select v-model="appState.sharedDataStatusFilter" @change="resetServicesPage('shared-data')">
                <option v-for="item in sharedDataStatusOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field query-field-wide">
              <span>关键字</span>
              <input v-model="appState.sharedDataKeyword" placeholder="请输入数据名称、表名称、申请单号或服务名称" @input="resetServicesPage('shared-data')" />
            </label>
          </div>

          <div v-else class="query-form-grid">
            <label class="query-field">
              <span>调用方式</span>
              <select v-model="appState.sharedCallMethodFilter" @change="resetServicesPage('share-calls')">
                <option v-for="item in sharedCallMethodOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field">
              <span>调用状态</span>
              <select v-model="appState.sharedCallStatusFilter" @change="resetServicesPage('share-calls')">
                <option v-for="item in sharedCallStatusOptions" :key="item" :value="item">{{ item }}</option>
              </select>
            </label>
            <label class="query-field query-field-wide">
              <span>关键字</span>
              <input v-model="appState.sharedCallKeyword" placeholder="请输入数据名称、申请单号、调用单位或服务名称" @input="resetServicesPage('share-calls')" />
            </label>
          </div>
        </section>

        <div class="content-grid">
          <section class="surface span-12">
            <div class="section-head">
              <div class="section-head-main">
                <p class="eyebrow">
                  {{
                    appState.shareModule === "access"
                      ? "接入清单"
                      : activeShareTertiaryId === "share-review"
                        ? "审核清单"
                        : activeShareTertiaryId === "shared-data"
                          ? "共享清单"
                          : activeShareTertiaryId === "share-calls"
                            ? "调用记录"
                            : "申请清单"
                  }}
                </p>
                <h3>
                  {{
                    appState.shareModule === "access"
                      ? "数据接入"
                      : activeShareTertiaryId === "share-review"
                        ? "共享审核"
                        : activeShareTertiaryId === "shared-data"
                          ? "共享清单"
                          : activeShareTertiaryId === "share-calls"
                            ? "数据调用"
                            : "数据共享"
                  }}
                </h3>
              </div>
              <div v-if="appState.shareModule === 'share' && activeShareTertiaryId === 'share-list'" class="table-toolbar">
                <button class="primary-button" @click="openShareCreate()">
                  <Send :size="18" />
                  发起共享申请
                </button>
              </div>
              <span v-else-if="appState.shareModule === 'share' && activeShareTertiaryId === 'shared-data'" class="caption">
                共 {{ filteredSharedDataItems.length }} 项
              </span>
              <span v-else-if="appState.shareModule === 'share' && activeShareTertiaryId === 'share-calls'" class="caption">
                共 {{ filteredSharedCallItems.length }} 条
              </span>
              <span v-else class="caption">共 {{ filteredAccessItems.length }} 项</span>
            </div>
            <div class="table-status-inline" aria-label="状态统计">
              <span v-for="item in currentServicesStatusSummary" :key="`services-inline-${item.title}`" class="table-status-item">
                <span>{{ item.title }}</span>
                <strong>{{ item.value }}</strong>
              </span>
            </div>
            <div v-if="appState.shareModule === 'access'" class="table-wrap">
              <table class="data-table access-table">
                <colgroup>
                  <col style="width: 24%" />
                  <col style="width: 14%" />
                  <col style="width: 22%" />
                  <col style="width: 12%" />
                  <col style="width: 12%" />
                  <col style="width: 8%" />
                  <col style="width: 8%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>数据名称</th>
                    <th>数据类型</th>
                    <th>来源渠道</th>
                    <th>接入时间</th>
                    <th>更新频率</th>
                    <th>状态</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in pagedAccessItems" :key="item.id">
                    <td><strong>{{ item.name }}</strong></td>
                    <td>{{ item.dataType }}</td>
                    <td>{{ item.sourceChannel }}</td>
                    <td>{{ item.accessDate }}</td>
                    <td>{{ item.refresh }}</td>
                    <td><span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span></td>
                    <td>
                      <button class="action-link" @click="openAccessDetail(item.id)">查看</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-if="appState.shareModule === 'access'" class="table-footer">
              <div class="table-footer-meta">
                <span>第 {{ currentServicesPageStart }}-{{ currentServicesPageEnd }} 条</span>
                <span>共 {{ currentServicesTotal }} 条</span>
              </div>
              <div class="table-pagination">
                <label class="page-size-select">
                  <span>每页</span>
                  <select :value="appState.servicesPageSize" @change="setServicesPageSize($event.target.value)">
                    <option v-for="item in servicesPageSizeOptions" :key="`size-${item}`" :value="item">{{ item }}</option>
                  </select>
                  <span>条</span>
                </label>
                <span class="page-indicator">第 {{ currentServicesPage }} / {{ currentServicesPageCount }} 页</span>
                <button class="secondary-button" :disabled="currentServicesPage <= 1" @click="changeServicesPage(-1)">上一页</button>
                <button class="secondary-button" :disabled="currentServicesPage >= currentServicesPageCount" @click="changeServicesPage(1)">下一页</button>
              </div>
            </div>
            <div v-else-if="activeShareTertiaryId === 'share-list'" class="table-wrap">
              <table class="data-table share-list-table">
                <colgroup>
                  <col style="width: 12%" />
                  <col style="width: 24%" />
                  <col style="width: 10%" />
                  <col style="width: 12%" />
                  <col style="width: 8%" />
                  <col style="width: 11%" />
                  <col style="width: 11%" />
                  <col style="width: 6%" />
                  <col style="width: 6%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>申请单号</th>
                    <th>服务名称</th>
                    <th>共享类型</th>
                    <th>申请单位</th>
                    <th>申请人</th>
                    <th>申请时间</th>
                    <th>当前环节</th>
                    <th>状态</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in pagedShareApplications" :key="item.id">
                    <td><strong>{{ item.applicationNo }}</strong></td>
                    <td>
                      <strong>{{ item.serviceName }}</strong>
                      <div class="caption table-subtext">{{ item.scopeDescription }}</div>
                    </td>
                    <td>{{ item.shareType }}</td>
                    <td>{{ item.applyUnit }}</td>
                    <td>{{ item.applicant }}</td>
                    <td>{{ item.applyTime }}</td>
                    <td>{{ item.currentStep }}</td>
                    <td><span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span></td>
                    <td>
                      <button class="action-link" @click="openShareDetail(item.id)">查看</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-else-if="activeShareTertiaryId === 'share-list'" class="table-footer">
              <div class="table-footer-meta">
                <span>第 {{ currentServicesPageStart }}-{{ currentServicesPageEnd }} 条</span>
                <span>共 {{ currentServicesTotal }} 条</span>
              </div>
              <div class="table-pagination">
                <label class="page-size-select">
                  <span>每页</span>
                  <select :value="appState.servicesPageSize" @change="setServicesPageSize($event.target.value)">
                    <option v-for="item in servicesPageSizeOptions" :key="`size-${item}`" :value="item">{{ item }}</option>
                  </select>
                  <span>条</span>
                </label>
                <span class="page-indicator">第 {{ currentServicesPage }} / {{ currentServicesPageCount }} 页</span>
                <button class="secondary-button" :disabled="currentServicesPage <= 1" @click="changeServicesPage(-1)">上一页</button>
                <button class="secondary-button" :disabled="currentServicesPage >= currentServicesPageCount" @click="changeServicesPage(1)">下一页</button>
              </div>
            </div>
            <div v-else-if="activeShareTertiaryId === 'share-review'" class="table-wrap">
              <table class="data-table share-review-table">
                <colgroup>
                  <col style="width: 14%" />
                  <col style="width: 26%" />
                  <col style="width: 16%" />
                  <col style="width: 14%" />
                  <col style="width: 12%" />
                  <col style="width: 10%" />
                  <col style="width: 4%" />
                  <col style="width: 4%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>申请单号</th>
                    <th>服务名称</th>
                    <th>申请单位</th>
                    <th>当前环节</th>
                    <th>当前处理人</th>
                    <th>申请时间</th>
                    <th>状态</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in pagedShareReviewApplications" :key="item.id">
                    <td><strong>{{ item.applicationNo }}</strong></td>
                    <td>{{ item.serviceName }}</td>
                    <td>{{ item.applyUnit }}</td>
                    <td>{{ item.currentStep }}</td>
                    <td>{{ item.currentReviewer }}</td>
                    <td>{{ item.applyTime }}</td>
                    <td><span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span></td>
                    <td>
                      <button class="action-link" @click="openShareReviewDetail(item.id)">查看</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-else-if="activeShareTertiaryId === 'share-review'" class="table-footer">
              <div class="table-footer-meta">
                <span>第 {{ currentServicesPageStart }}-{{ currentServicesPageEnd }} 条</span>
                <span>共 {{ currentServicesTotal }} 条</span>
              </div>
              <div class="table-pagination">
                <label class="page-size-select">
                  <span>每页</span>
                  <select :value="appState.servicesPageSize" @change="setServicesPageSize($event.target.value)">
                    <option v-for="item in servicesPageSizeOptions" :key="`size-${item}`" :value="item">{{ item }}</option>
                  </select>
                  <span>条</span>
                </label>
                <span class="page-indicator">第 {{ currentServicesPage }} / {{ currentServicesPageCount }} 页</span>
                <button class="secondary-button" :disabled="currentServicesPage <= 1" @click="changeServicesPage(-1)">上一页</button>
                <button class="secondary-button" :disabled="currentServicesPage >= currentServicesPageCount" @click="changeServicesPage(1)">下一页</button>
              </div>
            </div>
            <div v-else-if="activeShareTertiaryId === 'shared-data'" class="table-wrap">
              <table class="data-table shared-data-table">
                <colgroup>
                  <col style="width: 24%" />
                  <col style="width: 20%" />
                  <col style="width: 14%" />
                  <col style="width: 12%" />
                  <col style="width: 14%" />
                  <col style="width: 8%" />
                  <col style="width: 8%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>数据名称</th>
                    <th>表名称</th>
                    <th>数据共享方式</th>
                    <th>共享数据条数</th>
                    <th>关联申请单</th>
                    <th>开通状态</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in pagedSharedDataItems" :key="item.id">
                    <td><strong>{{ item.dataName }}</strong></td>
                    <td>{{ item.tableName }}</td>
                    <td>{{ item.shareMethod }}</td>
                    <td>{{ item.rowCount }}</td>
                    <td>{{ item.requestNo }}</td>
                    <td><span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span></td>
                    <td>
                      <button class="action-link" @click="openSharedDataDetail(item.id)">查看</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-else class="table-wrap">
              <table class="data-table share-call-table">
                <colgroup>
                  <col style="width: 18%" />
                  <col style="width: 13%" />
                  <col style="width: 14%" />
                  <col style="width: 10%" />
                  <col style="width: 14%" />
                  <col style="width: 14%" />
                  <col style="width: 9%" />
                  <col style="width: 8%" />
                </colgroup>
                <thead>
                  <tr>
                    <th>数据名称</th>
                    <th>调用单位</th>
                    <th>调用方式</th>
                    <th>调用状态</th>
                    <th>调用开始时间</th>
                    <th>调用结束时间</th>
                    <th>共享数据量</th>
                    <th>申请单号</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in pagedSharedCallItems" :key="item.id">
                    <td>
                      <strong>{{ item.dataName }}</strong>
                      <div class="caption table-subtext">{{ item.serviceName }}</div>
                    </td>
                    <td>{{ item.callerUnit }}</td>
                    <td>{{ item.callMethod }}</td>
                    <td><span class="badge" :class="badgeTone(item.callStatus)">{{ item.callStatus }}</span></td>
                    <td>{{ item.callStartTime }}</td>
                    <td>{{ item.callEndTime }}</td>
                    <td>{{ item.sharedVolume }}</td>
                    <td>{{ item.requestNo }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-if="appState.shareModule === 'share' && (activeShareTertiaryId === 'shared-data' || activeShareTertiaryId === 'share-calls')" class="table-footer">
              <div class="table-footer-meta">
                <span>第 {{ currentServicesPageStart }}-{{ currentServicesPageEnd }} 条</span>
                <span>共 {{ currentServicesTotal }} 条</span>
              </div>
              <div class="table-pagination">
                <label class="page-size-select">
                  <span>每页</span>
                  <select :value="appState.servicesPageSize" @change="setServicesPageSize($event.target.value)">
                    <option v-for="item in servicesPageSizeOptions" :key="`size-${item}`" :value="item">{{ item }}</option>
                  </select>
                  <span>条</span>
                </label>
                <span class="page-indicator">第 {{ currentServicesPage }} / {{ currentServicesPageCount }} 页</span>
                <button class="secondary-button" :disabled="currentServicesPage <= 1" @click="changeServicesPage(-1)">上一页</button>
                <button class="secondary-button" :disabled="currentServicesPage >= currentServicesPageCount" @click="changeServicesPage(1)">下一页</button>
              </div>
            </div>
          </section>
        </div>
      </section>

      <section v-if="appState.view === 'access-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToAccessList">
                <ChevronLeft :size="16" />
                返回接入清单
              </button>
              <p class="eyebrow">接入详情</p>
              <h3>{{ selectedAccessItem?.name }}</h3>
            </div>
          </div>
          <div v-if="selectedAccessItem" class="detail-content">
            <div class="detail-summary">
              <span class="badge primary">{{ selectedAccessItem.dataType }}</span>
              <span class="pill">{{ selectedAccessItem.status }}</span>
              <span class="pill">{{ selectedAccessItem.refresh }}</span>
            </div>
            <div class="detail-grid">
              <div class="detail-item"><span>数据名称</span><strong>{{ selectedAccessItem.name }}</strong></div>
              <div class="detail-item"><span>数据类型</span><strong>{{ selectedAccessItem.dataType }}</strong></div>
              <div class="detail-item"><span>来源渠道</span><strong>{{ selectedAccessItem.sourceChannel }}</strong></div>
              <div class="detail-item"><span>接入方式</span><strong>{{ selectedAccessItem.accessMode }}</strong></div>
              <div class="detail-item"><span>接入时间</span><strong>{{ selectedAccessItem.accessDate }}</strong></div>
              <div class="detail-item"><span>数据量</span><strong>{{ selectedAccessItem.dataVolume }}</strong></div>
              <div class="detail-item"><span>更新频率</span><strong>{{ selectedAccessItem.refresh }}</strong></div>
              <div class="detail-item"><span>当前状态</span><strong>{{ selectedAccessItem.status }}</strong></div>
              <div class="detail-item full-span"><span>来源系统</span><strong class="detail-value">{{ selectedAccessItem.originSystem }}</strong></div>
            </div>
            <div class="detail-block">
              <h4>每日接入记录</h4>
              <div class="table-wrap compact-table-wrap">
                <table class="data-table access-record-table">
                  <colgroup>
                    <col style="width: 18%" />
                    <col style="width: 11%" />
                    <col style="width: 14%" />
                    <col style="width: 12%" />
                    <col style="width: 12%" />
                    <col style="width: 14%" />
                    <col style="width: 14%" />
                    <col style="width: 5%" />
                  </colgroup>
                  <thead>
                    <tr>
                      <th>表名</th>
                      <th>更新日期</th>
                      <th>每天更新时间</th>
                      <th>更新数据量</th>
                      <th>当前累计数据量</th>
                      <th>任务开始时间</th>
                      <th>任务结束时间</th>
                      <th>状态</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="record in selectedAccessItem.dailyRecords" :key="`${selectedAccessItem.id}-${record.tableName}-${record.updateDate}`">
                      <td><strong>{{ record.tableName }}</strong></td>
                      <td>{{ record.updateDate }}</td>
                      <td>{{ record.updateTime }}</td>
                      <td>{{ record.updatedRows }}</td>
                      <td>{{ record.totalRows }}</td>
                      <td>{{ record.taskStartTime }}</td>
                      <td>{{ record.taskEndTime }}</td>
                      <td><span class="badge" :class="badgeTone(record.taskStatus)">{{ record.taskStatus }}</span></td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'share-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToShareList">
                <ChevronLeft :size="16" />
                返回申请清单
              </button>
              <p class="eyebrow">共享详情</p>
              <h3>{{ selectedShareApplication?.applicationNo }}</h3>
            </div>
            <div class="toolbar-actions">
              <button class="action-link" @click="openShareReviewDetail(selectedShareApplication?.id)">查看审核流程</button>
              <button class="action-link" @click="openSharedDataFromApplication(selectedShareApplication?.id)">查看共享清单</button>
            </div>
          </div>
          <div v-if="selectedShareApplication" class="detail-content">
            <div class="detail-summary">
              <span class="badge primary">{{ selectedShareApplication.shareType }}</span>
              <span class="pill">{{ selectedShareApplication.status }}</span>
              <span class="pill">{{ selectedShareApplication.currentStep }}</span>
            </div>
            <div class="detail-grid">
              <div class="detail-item"><span>服务名称</span><strong>{{ selectedShareApplication.serviceName }}</strong></div>
              <div class="detail-item"><span>数据共享方式</span><strong>{{ selectedShareApplication.shareMethod }}</strong></div>
              <div class="detail-item full-span"><span>数据描述</span><strong class="detail-value">{{ selectedShareApplication.dataDescription }}</strong></div>
              <div class="detail-item full-span"><span>应用范围描述</span><strong class="detail-value">{{ selectedShareApplication.scopeDescription }}</strong></div>
              <div class="detail-item"><span>共享频率</span><strong>{{ selectedShareApplication.shareFrequency }}</strong></div>
              <div class="detail-item"><span>涉密等级</span><strong>{{ selectedShareApplication.secrecyLevel }}</strong></div>
              <div class="detail-item"><span>责任单位</span><strong>{{ selectedShareApplication.ownerDept }}</strong></div>
              <div class="detail-item"><span>申请单位</span><strong>{{ selectedShareApplication.applyUnit }}</strong></div>
              <div class="detail-item"><span>申请人</span><strong>{{ selectedShareApplication.applicant }}</strong></div>
              <div class="detail-item"><span>申请时间</span><strong>{{ selectedShareApplication.applyTime }}</strong></div>
            </div>
            <div class="detail-block">
              <div class="detail-block-head">
                <h4>来源服务</h4>
                <button class="action-link" @click="openServiceDetail(selectedShareApplication.serviceId)">查看服务详情</button>
              </div>
              <p>{{ selectedShareSourceService?.detail }}</p>
            </div>
            <div class="detail-block">
              <h4>关联业务系统</h4>
              <div class="jump-list">
                <button v-for="item in selectedShareRelations.systems" :key="item.id" class="jump-pill" @click="jumpToSystem(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联数据资产</h4>
              <div class="jump-list">
                <button v-for="item in selectedShareRelations.assets" :key="item.id" class="jump-pill" @click="jumpToAsset(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联指标</h4>
              <div class="jump-list">
                <button v-for="item in selectedShareRelations.metrics" :key="item.id" class="jump-pill" @click="jumpToMetric(item.id)">{{ item.name }}</button>
              </div>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'share-create'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToShareList">
                <ChevronLeft :size="16" />
                返回申请清单
              </button>
              <p class="eyebrow">新建共享申请</p>
              <h3>配置数据共享申请单</h3>
            </div>
          </div>
          <form class="detail-content request-form" @submit.prevent="submitShareRequest">
            <div class="form-row">
              <label>
                <span>服务名称</span>
                <select v-model="shareRequestForm.serviceId" @change="applyShareRequestPreset(shareRequestForm.serviceId)">
                  <option v-for="item in serviceCatalog" :key="item.id" :value="item.id">{{ item.name }}</option>
                </select>
              </label>
              <label>
                <span>共享类型</span>
                <select v-model="shareRequestForm.shareType">
                  <option v-for="item in shareTypeOptions.filter((entry) => entry !== '全部')" :key="item" :value="item">{{ item }}</option>
                </select>
              </label>
            </div>
            <label>
              <span>数据描述</span>
              <textarea v-model="shareRequestForm.dataDescription" rows="4"></textarea>
            </label>
            <label>
              <span>应用范围描述</span>
              <textarea v-model="shareRequestForm.scopeDescription" rows="4"></textarea>
            </label>
            <div class="form-row">
              <label>
                <span>数据共享方式</span>
                <select v-model="shareRequestForm.shareMethod">
                  <option v-for="item in shareMethodOptions" :key="item" :value="item">{{ item }}</option>
                </select>
              </label>
              <label>
                <span>共享频率</span>
                <input v-model="shareRequestForm.shareFrequency" />
              </label>
            </div>
            <div class="form-row">
              <label>
                <span>涉密等级</span>
                <select v-model="shareRequestForm.secrecyLevel">
                  <option v-for="item in secrecyLevelOptions" :key="item" :value="item">{{ item }}</option>
                </select>
              </label>
              <label>
                <span>责任单位</span>
                <input v-model="shareRequestForm.ownerDept" />
              </label>
            </div>
            <div class="form-row">
              <label>
                <span>申请单位</span>
                <input v-model="shareRequestForm.applyUnit" />
              </label>
              <label>
                <span>申请人</span>
                <input v-model="shareRequestForm.applicant" />
              </label>
            </div>
            <label>
              <span>申请时间</span>
              <input v-model="shareRequestForm.applyTime" />
            </label>
            <div class="form-actions">
              <button type="submit" class="primary-button">
                <Send :size="18" />
                提交共享申请
              </button>
              <button type="button" class="action-link" @click="openServiceDetail(shareRequestForm.serviceId)">查看服务说明</button>
              <span class="caption">{{ feedback }}</span>
            </div>
          </form>
        </section>
      </section>

      <section v-if="appState.view === 'share-review-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToShareReview">
                <ChevronLeft :size="16" />
                返回审核页面
              </button>
              <p class="eyebrow">审核详情</p>
              <h3>{{ selectedShareApplication?.applicationNo }}</h3>
            </div>
            <div class="toolbar-actions">
              <button
                v-if="selectedShareApplication?.status !== '已开通'"
                class="primary-button"
                @click="approveShareRequest(selectedShareApplication?.id)"
              >
                <Send :size="18" />
                审核通过并开通共享
              </button>
              <button v-else class="action-link" @click="openSharedDataFromApplication(selectedShareApplication?.id)">查看共享清单</button>
            </div>
          </div>
          <div v-if="selectedShareApplication" class="detail-content">
            <div class="detail-summary">
              <span class="badge primary">{{ selectedShareApplication.shareType }}</span>
              <span class="pill">{{ selectedShareApplication.status }}</span>
              <span class="pill">{{ selectedShareApplication.currentStep }}</span>
            </div>
            <div class="detail-grid">
              <div class="detail-item"><span>服务名称</span><strong>{{ selectedShareApplication.serviceName }}</strong></div>
              <div class="detail-item"><span>当前处理人</span><strong>{{ selectedShareApplication.currentReviewer }}</strong></div>
              <div class="detail-item"><span>申请单位</span><strong>{{ selectedShareApplication.applyUnit }}</strong></div>
              <div class="detail-item"><span>申请人</span><strong>{{ selectedShareApplication.applicant }}</strong></div>
              <div class="detail-item"><span>申请时间</span><strong>{{ selectedShareApplication.applyTime }}</strong></div>
              <div class="detail-item"><span>共享方式</span><strong>{{ selectedShareApplication.shareMethod }}</strong></div>
            </div>
            <div class="detail-block">
              <h4>审核流程</h4>
              <div class="timeline">
                <article v-for="(item, index) in selectedShareApplication.reviewTimeline" :key="item.step" class="timeline-item">
                  <div class="timeline-index">{{ index + 1 }}</div>
                  <div class="detail-content">
                    <div class="signal-meta">
                      <strong>{{ item.step }}</strong>
                      <span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span>
                    </div>
                    <p>{{ item.detail }}</p>
                    <span class="caption">{{ item.time }}</span>
                  </div>
                </article>
              </div>
            </div>
            <div class="detail-block">
              <h4>审核说明</h4>
              <p>{{ feedback }}</p>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'shared-data-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToSharedDataList">
                <ChevronLeft :size="16" />
                返回共享清单
              </button>
              <p class="eyebrow">共享清单详情</p>
              <h3>{{ selectedSharedData?.dataName }}</h3>
            </div>
            <div class="toolbar-actions">
              <button class="action-link" @click="openShareDetail(selectedSharedDataRequest?.id)">查看申请单</button>
            </div>
          </div>
          <div v-if="selectedSharedData" class="detail-content">
            <div class="detail-summary">
              <span class="badge primary">{{ selectedSharedData.shareMethod }}</span>
              <span class="pill">{{ selectedSharedData.status }}</span>
              <span class="pill">{{ selectedSharedData.openedTime }}</span>
            </div>
            <div class="detail-grid">
              <div class="detail-item"><span>数据名称</span><strong>{{ selectedSharedData.dataName }}</strong></div>
              <div class="detail-item"><span>表名称</span><strong>{{ selectedSharedData.tableName }}</strong></div>
              <div class="detail-item"><span>数据共享方式</span><strong>{{ selectedSharedData.shareMethod }}</strong></div>
              <div class="detail-item"><span>共享数据条数</span><strong>{{ selectedSharedData.rowCount }}</strong></div>
              <div class="detail-item"><span>关联申请单</span><strong>{{ selectedSharedData.requestNo }}</strong></div>
              <div class="detail-item"><span>来源服务</span><strong>{{ selectedSharedData.serviceName }}</strong></div>
              <div class="detail-item full-span"><span>表下载</span><strong class="detail-value">{{ selectedSharedData.downloadDetail }}</strong></div>
              <div class="detail-item"><span>接口地址</span><strong>{{ selectedSharedData.interfaceDetail.endpoint }}</strong></div>
              <div class="detail-item"><span>调用方式</span><strong>{{ selectedSharedData.interfaceDetail.method }}</strong></div>
              <div class="detail-item"><span>鉴权方式</span><strong>{{ selectedSharedData.interfaceDetail.auth }}</strong></div>
              <div class="detail-item full-span"><span>接口推送说明</span><strong class="detail-value">{{ selectedSharedData.interfaceDetail.request }}</strong></div>
              <div class="detail-item full-span"><span>接口返回说明</span><strong class="detail-value">{{ selectedSharedData.interfaceDetail.response }}</strong></div>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'service-detail'" class="view active">
        <section class="surface detail-panel">
          <div class="section-head">
            <div>
              <button class="back-button" @click="backToServiceList">
                <ChevronLeft :size="16" />
                返回数据共享
              </button>
              <p class="eyebrow">服务详情</p>
              <h3>{{ selectedService?.name }}</h3>
            </div>
          </div>
          <div v-if="selectedService" class="detail-content">
            <div class="detail-summary">
              <span class="badge primary">{{ selectedService.type }}</span>
              <span class="pill">{{ selectedService.status }}</span>
              <span class="pill">{{ selectedService.shareStatus }}</span>
            </div>
            <div class="detail-grid">
              <div class="detail-item"><span>服务名称</span><strong>{{ selectedService.name }}</strong></div>
              <div class="detail-item"><span>提供方</span><strong>{{ selectedService.provider }}</strong></div>
              <div class="detail-item"><span>责任处室</span><strong>{{ selectedService.ownerDept }}</strong></div>
              <div class="detail-item"><span>服务对象</span><strong>{{ selectedService.target }}</strong></div>
              <div class="detail-item"><span>授权方式</span><strong>{{ selectedService.auth }}</strong></div>
              <div class="detail-item"><span>更新频率</span><strong>{{ selectedService.refresh }}</strong></div>
              <div class="detail-item"><span>服务状态</span><strong>{{ selectedService.status }}</strong></div>
              <div class="detail-item"><span>共享状态</span><strong>{{ selectedService.shareStatus }}</strong></div>
              <div class="detail-item full-span"><span>服务说明</span><strong class="detail-value">{{ selectedService.detail }}</strong></div>
            </div>
            <div class="detail-block">
              <div class="detail-block-head">
                <h4>共享申请</h4>
                <button class="primary-button" @click="openShareCreate(selectedService?.id)">
                  <Send :size="18" />
                  发起共享申请
                </button>
              </div>
              <p>按当前服务生成共享申请单，提交后进入“责任单位确认 - 数据办审核 - 共享开通”流程。</p>
            </div>
            <div class="detail-block">
              <h4>关联业务系统</h4>
              <div class="jump-list">
                <button v-for="item in selectedServiceRelations.systems" :key="item.id" class="jump-pill" @click="jumpToSystem(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联数据资产</h4>
              <div class="jump-list">
                <button v-for="item in selectedServiceRelations.assets" :key="item.id" class="jump-pill" @click="jumpToAsset(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联指标</h4>
              <div class="jump-list">
                <button v-for="item in selectedServiceRelations.metrics" :key="item.id" class="jump-pill" @click="jumpToMetric(item.id)">{{ item.name }}</button>
              </div>
            </div>
            <div class="detail-block">
              <h4>关联申请记录</h4>
              <div class="request-stack">
                <article v-for="item in serviceRelatedApplications" :key="item.id" class="request-item">
                  <div class="signal-meta">
                    <strong>{{ item.applicationNo }}</strong>
                    <span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span>
                  </div>
                  <p>{{ item.applyUnit }} · {{ item.applicant }} · {{ item.applyTime }}</p>
                  <div class="toolbar-actions">
                    <span class="caption">{{ item.currentStep }}</span>
                    <button class="action-link" @click="openShareDetail(item.id)">查看申请</button>
                  </div>
                </article>
              </div>
            </div>
          </div>
        </section>
      </section>

      <section v-if="appState.view === 'analysis'" class="view active">
        <div class="section-toolbar">
          <div class="toolbar-context">
            <span class="toolbar-label">三级菜单</span>
            <strong>{{ analysisModules.find((item) => item.id === appState.analysisModule)?.label }}</strong>
          </div>
          <div v-if="appState.analysisModule === 'business'" class="toolbar-actions">
            <div class="filter-chips">
              <button
                v-for="item in businessBoardModules"
                :key="item.id"
                class="chip"
                :class="{ active: appState.businessBoardModule === item.id }"
                @click="selectBusinessBoardModule(item.id)"
              >
                {{ item.label }}
              </button>
            </div>
          </div>
        </div>

        <div v-if="appState.analysisModule === 'access'" class="content-grid">
          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">接入趋势</p>
                <h3>接入数据总量趋势</h3>
              </div>
            </div>
            <div class="bar-chart">
              <article
                v-for="item in accessTrend"
                :key="item.month"
                class="bar-item"
                @mousemove="showAnalysisTooltip($event, item.month, `接入数据总量：${item.value}`)"
                @mouseleave="hideAnalysisTooltip"
              >
                <div class="bar-track">
                  <div class="bar-fill access" :style="{ height: `${(item.value / accessTrendMax) * 100}%` }"></div>
                </div>
                <strong>{{ item.value }}</strong>
                <span>{{ item.month }}</span>
              </article>
            </div>
          </section>

          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">共享趋势</p>
                <h3>共享数据总量趋势</h3>
              </div>
            </div>
            <div class="bar-chart">
              <article
                v-for="item in shareTrend"
                :key="item.month"
                class="bar-item"
                @mousemove="showAnalysisTooltip($event, item.month, `共享数据总量：${item.value}`)"
                @mouseleave="hideAnalysisTooltip"
              >
                <div class="bar-track">
                  <div class="bar-fill share" :style="{ height: `${(item.value / shareTrendMax) * 100}%` }"></div>
                </div>
                <strong>{{ item.value }}</strong>
                <span>{{ item.month }}</span>
              </article>
            </div>
          </section>

          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">共享分布</p>
                <h3>共享类型与共享单位</h3>
              </div>
            </div>
            <div class="donut-grid donut-grid-two">
              <section class="donut-panel">
                <div class="donut-chart-shell">
                  <svg class="donut-chart" viewBox="0 0 156 156" aria-label="共享类型分布">
                    <path
                      v-for="segment in shareTypeSegments"
                      :key="segment.label"
                      :d="segment.path"
                      :fill="segment.color"
                      @mousemove="showAnalysisTooltip($event, segment.label, `共享类型：${segment.value}`)"
                      @mouseleave="hideAnalysisTooltip"
                    />
                  </svg>
                  <div class="donut-center">
                    <strong>{{ serviceCatalog.length }}</strong>
                    <span>共享事项</span>
                  </div>
                </div>
                <div class="donut-legend">
                  <article
                    v-for="segment in shareTypeSegments"
                    :key="segment.label"
                    class="legend-item"
                    @mousemove="showAnalysisTooltip($event, segment.label, `共享类型：${segment.value}`)"
                    @mouseleave="hideAnalysisTooltip"
                  >
                    <span class="legend-swatch" :style="{ background: segment.color }"></span>
                    <span>{{ segment.label }}</span>
                    <strong>{{ segment.value }}</strong>
                  </article>
                </div>
              </section>

              <section class="donut-panel">
                <div class="donut-chart-shell">
                  <svg class="donut-chart" viewBox="0 0 156 156" aria-label="共享单位分布">
                    <path
                      v-for="segment in shareUnitSegments"
                      :key="segment.label"
                      :d="segment.path"
                      :fill="segment.color"
                      @mousemove="showAnalysisTooltip($event, segment.label, `共享单位：${segment.value}`)"
                      @mouseleave="hideAnalysisTooltip"
                    />
                  </svg>
                  <div class="donut-center">
                    <strong>{{ shareUnitDistribution.length }}</strong>
                    <span>共享单位</span>
                  </div>
                </div>
                <div class="donut-legend">
                  <article
                    v-for="segment in shareUnitSegments"
                    :key="segment.label"
                    class="legend-item"
                    @mousemove="showAnalysisTooltip($event, segment.label, `共享单位：${segment.value}`)"
                    @mouseleave="hideAnalysisTooltip"
                  >
                    <span class="legend-swatch" :style="{ background: segment.color }"></span>
                    <span>{{ segment.label }}</span>
                    <strong>{{ segment.value }}</strong>
                  </article>
                </div>
              </section>
            </div>
          </section>
        </div>

        <div v-else class="content-grid">
          <section class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">乡村旅游</p>
                <h3>全省村寨申报复核概况</h3>
              </div>
              <span class="caption">当前总量最高市州：{{ ruralTourismTopCity?.city }} · {{ ruralTourismTopCity?.total }}</span>
            </div>
            <div class="level-summary-grid">
              <article v-for="item in ruralTourismBoard.levelSummary" :key="item.level" class="level-summary-card">
                <div class="signal-meta">
                  <strong>{{ item.level }}</strong>
                  <span class="pill">{{ item.total }} 项</span>
                </div>
                <div class="status-metric-grid">
                  <div class="status-metric passed">
                    <span>通过</span>
                    <strong>{{ item.passed }}</strong>
                  </div>
                  <div class="status-metric failed">
                    <span>不通过</span>
                    <strong>{{ item.failed }}</strong>
                  </div>
                  <div class="status-metric pending">
                    <span>进行中</span>
                    <strong>{{ item.inProgress }}</strong>
                  </div>
                </div>
                <div class="stacked-progress">
                  <span class="progress-segment passed" :style="{ width: `${(item.passed / item.total) * 100}%` }"></span>
                  <span class="progress-segment failed" :style="{ width: `${(item.failed / item.total) * 100}%` }"></span>
                  <span class="progress-segment pending" :style="{ width: `${(item.inProgress / item.total) * 100}%` }"></span>
                </div>
              </article>
            </div>
          </section>

          <section v-if="appState.businessBoardModule === 'ruralTourism'" class="surface span-12">
            <div class="section-head">
              <div>
                <p class="eyebrow">市州分布</p>
                <h3>各市州不同等级不同状态统计</h3>
              </div>
              <div class="filter-chips">
                <button
                  v-for="item in businessBoardViews"
                  :key="item.id"
                  class="chip"
                  :class="{ active: appState.businessBoardView === item.id }"
                  @click="selectBusinessBoardView(item.id)"
                >
                  {{ item.label }}
                </button>
              </div>
            </div>
            <div v-if="appState.businessBoardView === 'matrix'" class="table-wrap">
              <table class="data-table rural-matrix-table">
                <thead>
                  <tr>
                    <th rowspan="2">市州</th>
                    <th colspan="4">甲级</th>
                    <th colspan="4">乙级</th>
                    <th colspan="4">丙级</th>
                    <th colspan="4">总计</th>
                  </tr>
                  <tr>
                    <th>通过</th>
                    <th>不通过</th>
                    <th>进行中</th>
                    <th>小计</th>
                    <th>通过</th>
                    <th>不通过</th>
                    <th>进行中</th>
                    <th>小计</th>
                    <th>通过</th>
                    <th>不通过</th>
                    <th>进行中</th>
                    <th>小计</th>
                    <th>通过</th>
                    <th>不通过</th>
                    <th>进行中</th>
                    <th>小计</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="item in ruralTourismBoard.citySummary" :key="item.city">
                    <td class="matrix-city-cell">
                      <strong>{{ item.city }}</strong>
                    </td>
                    <td class="numeric">{{ item.levels["甲级"].passed }}</td>
                    <td class="numeric">{{ item.levels["甲级"].failed }}</td>
                    <td class="numeric">{{ item.levels["甲级"].inProgress }}</td>
                    <td class="numeric emphasis">{{ item.levels["甲级"].total }}</td>
                    <td class="numeric">{{ item.levels["乙级"].passed }}</td>
                    <td class="numeric">{{ item.levels["乙级"].failed }}</td>
                    <td class="numeric">{{ item.levels["乙级"].inProgress }}</td>
                    <td class="numeric emphasis">{{ item.levels["乙级"].total }}</td>
                    <td class="numeric">{{ item.levels["丙级"].passed }}</td>
                    <td class="numeric">{{ item.levels["丙级"].failed }}</td>
                    <td class="numeric">{{ item.levels["丙级"].inProgress }}</td>
                    <td class="numeric emphasis">{{ item.levels["丙级"].total }}</td>
                    <td class="numeric">{{ item.passed }}</td>
                    <td class="numeric">{{ item.failed }}</td>
                    <td class="numeric">{{ item.inProgress }}</td>
                    <td class="numeric emphasis total">{{ item.total }}</td>
                  </tr>
                </tbody>
                <tfoot>
                  <tr class="table-total-row">
                    <td><strong>{{ ruralTourismProvinceRow.city }}</strong></td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["甲级"].passed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["甲级"].failed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["甲级"].inProgress }}</td>
                    <td class="numeric emphasis">{{ ruralTourismProvinceRow.levels["甲级"].total }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["乙级"].passed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["乙级"].failed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["乙级"].inProgress }}</td>
                    <td class="numeric emphasis">{{ ruralTourismProvinceRow.levels["乙级"].total }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["丙级"].passed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["丙级"].failed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.levels["丙级"].inProgress }}</td>
                    <td class="numeric emphasis">{{ ruralTourismProvinceRow.levels["丙级"].total }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.passed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.failed }}</td>
                    <td class="numeric">{{ ruralTourismProvinceRow.inProgress }}</td>
                    <td class="numeric emphasis total">{{ ruralTourismProvinceRow.total }}</td>
                  </tr>
                </tfoot>
              </table>
            </div>
            <div v-else class="report-grid">
              <article class="report-card report-card-wide">
                <div class="report-card-head">
                  <div>
                    <span class="caption">市州总量</span>
                    <h4>各市州申报总量对比</h4>
                  </div>
                </div>
                <div class="report-list">
                  <article
                    v-for="item in ruralTourismBoard.citySummary"
                    :key="`${item.city}-total-report`"
                    class="report-row"
                  >
                    <div class="report-row-head">
                      <strong>{{ item.city }}</strong>
                      <span>{{ item.total }} 项</span>
                    </div>
                    <div class="report-track">
                      <span
                        class="report-fill total"
                        :style="{ width: `${(item.total / ruralTourismMaxCityTotal) * 100}%` }"
                      ></span>
                    </div>
                    <div class="report-stack-legend">
                      <span class="passed">通过 {{ item.passed }}</span>
                      <span class="failed">不通过 {{ item.failed }}</span>
                      <span class="pending">进行中 {{ item.inProgress }}</span>
                    </div>
                  </article>
                </div>
              </article>

              <article
                v-for="panel in ruralTourismLevelReportPanels"
                :key="panel.level"
                class="report-card"
              >
                <div class="report-card-head">
                  <div>
                    <span class="caption">{{ panel.level }}</span>
                    <h4>{{ panel.level }}按市州状态分布</h4>
                  </div>
                </div>
                <div class="report-list">
                  <article v-for="row in panel.rows" :key="`${panel.level}-${row.city}`" class="report-row">
                    <div class="report-row-head">
                      <strong>{{ row.city }}</strong>
                      <span>{{ row.total }} 项</span>
                    </div>
                    <div class="report-track report-track-stack">
                      <span
                        class="report-fill passed"
                        :style="{ width: `${(row.passed / panel.max) * 100}%` }"
                      ></span>
                      <span
                        class="report-fill failed"
                        :style="{ width: `${(row.failed / panel.max) * 100}%` }"
                      ></span>
                      <span
                        class="report-fill pending"
                        :style="{ width: `${(row.inProgress / panel.max) * 100}%` }"
                      ></span>
                    </div>
                    <div class="report-stack-legend">
                      <span class="passed">通过 {{ row.passed }}</span>
                      <span class="failed">不通过 {{ row.failed }}</span>
                      <span class="pending">进行中 {{ row.inProgress }}</span>
                    </div>
                  </article>
                </div>
              </article>
            </div>
          </section>
        </div>
        <div v-if="analysisTooltip.visible" class="analysis-tooltip" :style="{ left: `${analysisTooltip.x}px`, top: `${analysisTooltip.y}px` }">
          <strong>{{ analysisTooltip.title }}</strong>
          <span>{{ analysisTooltip.meta }}</span>
        </div>
      </section>

      <section v-if="appState.view === 'permissions'" class="view active">
        <section class="surface permission-hero">
          <div class="permission-hero-copy">
            <p class="eyebrow">统一权限配置</p>
            <h3>围绕用户、角色、组织和数据范围进行统一授权</h3>
            <p>支持菜单页面权限、数据范围权限、字段脱敏权限和共享服务开通权限统一配置，并对在线登录情况进行可视化管理。</p>
          </div>
          <div class="permission-hero-grid">
            <article v-for="item in permissionOverviewCards" :key="item.title" class="permission-hero-card">
              <span>{{ item.title }}</span>
              <strong>{{ item.value }}</strong>
              <p>{{ item.meta }}</p>
            </article>
          </div>
        </section>

        <div v-if="appState.permissionModule === 'users'" class="content-grid">
          <section class="surface span-8">
            <div class="section-head">
              <div>
                <p class="eyebrow">用户与登录状态</p>
                <h3>用户管理</h3>
              </div>
            </div>
            <div class="table-wrap">
              <table class="data-table">
                <thead>
                  <tr>
                    <th>姓名</th>
                    <th>账号</th>
                    <th>所属组织</th>
                    <th>角色</th>
                    <th>状态</th>
                    <th>登录状态</th>
                    <th>最近登录时间</th>
                    <th>数据范围</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="item in userDirectory"
                    :key="item.id"
                    class="clickable-row"
                    :class="{ selected: selectedUser?.id === item.id }"
                    @click="selectUser(item.id)"
                  >
                    <td><strong>{{ item.name }}</strong></td>
                    <td>{{ item.account }}</td>
                    <td>{{ item.org }}</td>
                    <td>{{ item.role }}</td>
                    <td><span class="badge" :class="badgeTone(item.status)">{{ item.status }}</span></td>
                    <td><span class="badge" :class="item.loginState === '在线' ? 'primary' : item.status === '停用' ? 'neutral' : 'warn'">{{ item.loginState }}</span></td>
                    <td>{{ item.lastLogin }}</td>
                    <td>{{ item.scope }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>

          <section class="surface span-4 detail-panel">
            <div class="section-head">
              <div>
                <p class="eyebrow">用户详情</p>
                <h3>{{ selectedUser?.name }}</h3>
              </div>
            </div>
            <div v-if="selectedUser" class="detail-content">
              <div class="detail-summary">
                <span class="badge primary">{{ selectedUser.role }}</span>
                <span class="pill">{{ selectedUser.status }}</span>
                <span class="pill">{{ selectedUser.loginState }}</span>
              </div>
              <div class="detail-grid">
                <div class="detail-item"><span>账号</span><strong>{{ selectedUser.account }}</strong></div>
                <div class="detail-item"><span>所属组织</span><strong>{{ selectedUser.org }}</strong></div>
                <div class="detail-item"><span>当前角色</span><strong>{{ selectedUser.role }}</strong></div>
                <div class="detail-item"><span>数据范围</span><strong>{{ selectedUser.scope }}</strong></div>
                <div class="detail-item"><span>最近登录</span><strong>{{ selectedUser.lastLogin }}</strong></div>
                <div class="detail-item"><span>当前会话</span><strong>{{ selectedUser.currentSession }}</strong></div>
                <div class="detail-item"><span>登录终端</span><strong>{{ selectedUser.device }}</strong></div>
                <div class="detail-item"><span>登录 IP</span><strong>{{ selectedUser.loginIp }}</strong></div>
              </div>
              <div class="detail-block">
                <h4>功能模块</h4>
                <div class="detail-list">
                  <span v-for="menu in permissionMenus.find((item) => item.role === selectedUser.role)?.menus ?? []" :key="menu" class="pill">{{ menu }}</span>
                </div>
              </div>
            </div>
          </section>
        </div>

        <div v-else-if="appState.permissionModule === 'roles'" class="content-grid">
          <section class="surface span-4">
            <div class="section-head">
              <div>
                <p class="eyebrow">角色清单</p>
                <h3>角色管理</h3>
              </div>
            </div>
            <div class="permission-grid single-column">
              <article
                v-for="item in roleDirectory"
                :key="item.id"
                class="permission-card selectable-card"
                :class="{ active: selectedRole?.id === item.id }"
                @click="selectRole(item.id)"
              >
                <div class="permission-head">
                  <div>
                    <h4>{{ item.name }}</h4>
                    <p class="caption">组织范围：{{ item.orgScope }}</p>
                  </div>
                  <span class="badge neutral">{{ item.memberCount }} 人</span>
                </div>
                <p><strong>功能模块：</strong>{{ item.functionModules.length }} 项</p>
                <p><strong>数据权限：</strong>{{ item.dataPermissions.length }} 项</p>
              </article>
            </div>
          </section>

          <section class="surface span-8 detail-panel">
            <div class="section-head">
              <div>
                <p class="eyebrow">角色授权配置</p>
                <h3>{{ selectedRole?.name }}</h3>
              </div>
            </div>
            <div v-if="selectedRole" class="detail-content">
              <div class="permission-config-grid">
                <article class="permission-config-card">
                  <div class="detail-block-head">
                    <h4>角色基础信息</h4>
                    <span class="badge neutral">{{ selectedRole.memberCount }} 人</span>
                  </div>
                  <div class="detail-grid">
                    <div class="detail-item"><span>角色名称</span><strong>{{ selectedRole.name }}</strong></div>
                    <div class="detail-item"><span>组织范围</span><strong>{{ selectedRole.orgScope }}</strong></div>
                    <div class="detail-item full-span"><span>字段权限策略</span><strong class="detail-value">{{ selectedRole.fieldPolicy }}</strong></div>
                  </div>
                </article>
                <article class="permission-config-card">
                  <div class="detail-block-head">
                    <h4>功能页面配置</h4>
                    <span class="badge primary">{{ selectedRole.functionModules.length }} 项</span>
                  </div>
                  <div class="toggle-grid">
                    <button
                      v-for="item in permissionFunctionOptions"
                      :key="item"
                      class="chip toggle-chip"
                      :class="{ active: selectedRole.functionModules.includes(item) }"
                      @click="toggleRoleFunction(item)"
                    >
                      {{ item }}
                    </button>
                  </div>
                </article>
              </div>
              <div class="permission-config-grid">
                <article class="permission-config-card">
                  <div class="detail-block-head">
                    <h4>数据权限配置</h4>
                    <span class="badge primary">{{ selectedRole.dataPermissions.length }} 项</span>
                  </div>
                  <div class="toggle-grid">
                    <button
                      v-for="item in permissionDataOptions"
                      :key="item"
                      class="chip toggle-chip"
                      :class="{ active: selectedRole.dataPermissions.includes(item) }"
                      @click="toggleRoleDataPermission(item)"
                    >
                      {{ item }}
                    </button>
                  </div>
                </article>
                <article class="permission-config-card">
                  <div class="detail-block-head">
                    <h4>权限控制规则</h4>
                  </div>
                  <div class="permission-rule-list">
                    <article v-for="item in permissionRules" :key="item.type" class="permission-rule-item">
                      <strong>{{ item.type }}</strong>
                      <p>{{ item.policy }}</p>
                      <span class="caption">{{ item.object }}：{{ item.sample }}</span>
                    </article>
                  </div>
                </article>
              </div>
            </div>
          </section>
        </div>

        <div v-else class="content-grid">
          <section class="surface span-8">
            <div class="section-head">
              <div>
                <p class="eyebrow">组织清单</p>
                <h3>组织机构管理</h3>
              </div>
            </div>
            <div class="table-wrap">
              <table class="data-table">
                <thead>
                  <tr>
                    <th>组织名称</th>
                    <th>层级</th>
                    <th>上级组织</th>
                    <th>用户数</th>
                    <th>角色数</th>
                    <th>覆盖范围</th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="item in orgDirectory"
                    :key="item.id"
                    class="clickable-row"
                    :class="{ selected: selectedOrg?.id === item.id }"
                    @click="selectOrg(item.id)"
                  >
                    <td><strong>{{ item.name }}</strong></td>
                    <td>{{ item.level }}</td>
                    <td>{{ item.parent }}</td>
                    <td>{{ item.userCount }}</td>
                    <td>{{ item.roleCount }}</td>
                    <td>{{ item.coverage }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>

          <section class="surface span-4 detail-panel">
            <div class="section-head">
              <div>
                <p class="eyebrow">组织详情</p>
                <h3>{{ selectedOrg?.name }}</h3>
              </div>
            </div>
            <div v-if="selectedOrg" class="detail-content">
              <div class="detail-summary">
                <span class="badge primary">{{ selectedOrg.level }}</span>
                <span class="pill">{{ selectedOrg.coverage }}</span>
              </div>
              <div class="detail-grid">
                <div class="detail-item"><span>上级组织</span><strong>{{ selectedOrg.parent }}</strong></div>
                <div class="detail-item"><span>组织层级</span><strong>{{ selectedOrg.level }}</strong></div>
                <div class="detail-item"><span>用户数量</span><strong>{{ selectedOrg.userCount }}</strong></div>
                <div class="detail-item"><span>角色数量</span><strong>{{ selectedOrg.roleCount }}</strong></div>
                <div class="detail-item full-span"><span>覆盖范围</span><strong class="detail-value">{{ selectedOrg.coverage }}</strong></div>
              </div>
              <div class="detail-block">
                <h4>组织授权说明</h4>
                <p>组织机构用于承接角色授权和数据范围控制，支持按省级、处室、市州、景区单位逐级分配账户和菜单权限。</p>
              </div>
            </div>
          </section>
        </div>
      </section>

      <section v-if="appState.view === 'governance'" class="view active">
        <div class="content-grid">
          <section class="surface span-6">
            <div class="section-head">
              <div>
                <p class="eyebrow">治理策略</p>
                <h3>六个统一</h3>
              </div>
            </div>
            <div class="governance-grid">
              <article v-for="item in governanceItems" :key="item.title" class="governance-card">
                <h4>{{ item.title }}</h4>
                <p>{{ item.desc }}</p>
              </article>
            </div>
          </section>
          <section class="surface span-6">
            <div class="section-head">
              <div>
                <p class="eyebrow">技术架构</p>
                <h3>四层服务体系</h3>
              </div>
            </div>
            <div class="architecture-stack">
              <article v-for="item in architectureLayers" :key="item.title" class="layer-card">
                <div class="layer-head">
                  <Blocks :size="18" />
                  <h4>{{ item.title }}</h4>
                </div>
                <p>{{ item.desc }}</p>
              </article>
            </div>
          </section>
        </div>
      </section>
    </main>
  </div>
</template>
