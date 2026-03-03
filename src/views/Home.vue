<template>
  <div class="min-h-screen bg-white font-['PingFang_SC','Microsoft_YaHei',sans-serif] text-[#333]">
    <header class="sticky top-4 z-50 mx-auto w-[92%] max-w-7xl rounded-2xl border border-white/60 bg-white/75 px-4 shadow-lg shadow-slate-200/50 backdrop-blur md:px-8">
      <nav class="flex h-16 items-center justify-between">
        <a href="#" class="flex items-center gap-2 text-lg font-semibold text-slate-700">
          <div class="h-8 w-8 rounded-full bg-gradient-to-br from-[#E6F0FF] to-[#FFF0F5]"></div>
          优孕指南
        </a>
        <ul class="hidden items-center gap-7 text-sm text-slate-600 md:flex">
          <li v-for="item in navItems" :key="item" class="cursor-pointer transition-colors hover:text-slate-900">{{ item }}</li>
        </ul>
        <el-button type="primary" round class="!bg-slate-700 !border-slate-700 hover:!bg-slate-800">开始攻略</el-button>
      </nav>
    </header>

    <main class="space-y-24 pb-16">
      <section class="relative overflow-hidden px-4 pt-20 md:px-8">
        <div class="mx-auto grid max-w-7xl items-center gap-10 rounded-3xl bg-gradient-to-br from-[#E6F0FF] via-white to-[#FFF0F5] p-8 md:grid-cols-2 md:p-14">
          <div class="space-y-6 opacity-0 reveal-item">
            <p class="text-sm font-medium tracking-widest text-slate-500">优孕指南 · IVF 全流程科普</p>
            <h1 class="text-3xl font-semibold leading-tight text-slate-800 md:text-5xl">科学助孕，连接新生命</h1>
            <p class="max-w-xl text-sm leading-7 text-slate-600 md:text-base">以循证医学视角，为你梳理从术前评估到验孕复盘的关键节点，帮助家庭更从容地完成试管婴儿旅程。</p>
            <div class="w-full max-w-xl rounded-2xl border border-white/70 bg-white p-2 shadow-md shadow-slate-200/70">
              <el-input v-model="searchKeyword" size="large" placeholder="输入关键词，例如：促排方案 / 费用 / 医院选择">
                <template #append>
                  <el-button type="primary" class="!bg-slate-700 !border-slate-700">开始攻略</el-button>
                </template>
              </el-input>
            </div>
          </div>
          <div class="relative opacity-0 reveal-item">
            <div class="h-72 rounded-3xl bg-white/70 p-5 shadow-xl shadow-blue-100/60 md:h-96">
              <img
                src="https://images.unsplash.com/photo-1584515933487-779824d29309?auto=format&fit=crop&w=1200&q=80"
                alt="医疗咨询场景"
                class="h-full w-full rounded-2xl object-cover"
              />
            </div>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-7xl px-4 md:px-8">
        <div class="mb-8 opacity-0 reveal-item">
          <h2 class="text-2xl font-semibold text-slate-800 md:text-3xl">试管全流程步骤</h2>
          <p class="mt-3 text-slate-600">点击步骤卡，快速了解每个阶段目标与注意事项。</p>
        </div>

        <el-steps :active="activeStep" finish-status="success" align-center class="hidden md:flex">
          <el-step v-for="(step, index) in steps" :key="step.title" :title="step.title" @click="activeStep = index" />
        </el-steps>

        <div class="mt-8 grid gap-4 md:grid-cols-6">
          <button
            v-for="(step, index) in steps"
            :key="step.title"
            class="rounded-2xl border p-4 text-left transition-all duration-300"
            :class="index === activeStep ? 'border-slate-700 bg-slate-700 text-white shadow-lg' : 'border-slate-200 bg-white text-slate-700 hover:-translate-y-1 hover:shadow-md'"
            @click="activeStep = index"
          >
            <p class="text-xs opacity-80">STEP {{ index + 1 }}</p>
            <p class="mt-2 font-medium">{{ step.title }}</p>
          </button>
        </div>

        <transition name="fade-up" mode="out-in">
          <article
            :key="steps[activeStep].title"
            class="mt-6 rounded-3xl border border-slate-100 bg-white p-6 shadow-lg shadow-slate-100/80"
          >
            <h3 class="text-xl font-semibold text-slate-800">{{ steps[activeStep].title }}</h3>
            <p class="mt-3 leading-7 text-slate-600">{{ steps[activeStep].description }}</p>
          </article>
        </transition>
      </section>

      <section class="mx-auto max-w-7xl px-4 md:px-8">
        <div class="mb-8 opacity-0 reveal-item">
          <h2 class="text-2xl font-semibold text-slate-800 md:text-3xl">热门科普文章</h2>
        </div>
        <div class="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
          <article
            v-for="card in infoCards"
            :key="card.title"
            class="group rounded-3xl border border-slate-100 bg-white p-6 shadow-md shadow-slate-100 transition-all duration-300 hover:-translate-y-1 hover:scale-[1.01] hover:shadow-xl"
          >
            <component :is="card.icon" class="h-9 w-9 text-slate-600" />
            <h3 class="mt-4 text-lg font-semibold text-slate-800">{{ card.title }}</h3>
            <p class="mt-2 text-sm leading-6 text-slate-600">{{ card.desc }}</p>
            <a href="#" class="mt-5 inline-block text-sm font-medium text-slate-700 transition group-hover:text-slate-900">阅读全文 →</a>
          </article>
        </div>
      </section>

      <section class="mx-auto max-w-4xl px-4 md:px-8">
        <div class="mb-8 opacity-0 reveal-item text-center">
          <h2 class="text-2xl font-semibold text-slate-800 md:text-3xl">常见问题 FAQ</h2>
        </div>
        <el-collapse accordion>
          <el-collapse-item v-for="item in faqs" :key="item.title" :title="item.title" :name="item.title">
            <p class="leading-7 text-slate-600">{{ item.answer }}</p>
          </el-collapse-item>
        </el-collapse>
      </section>
    </main>

    <footer class="mt-20 border-t border-slate-100 bg-slate-50/70">
      <div class="mx-auto grid max-w-7xl gap-4 px-4 py-10 text-sm text-slate-600 md:grid-cols-3 md:px-8">
        <p>联系方式：400-888-2026 · support@yoyun-guide.com</p>
        <p>免责声明：内容仅供科普参考，具体诊疗请遵医嘱。</p>
        <p class="md:text-right">© {{ new Date().getFullYear() }} 优孕指南 · All Rights Reserved.</p>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { Activity, BadgeHelp, BookOpen, HeartPulse, Microscope, ShieldCheck } from 'lucide-vue-next'

const searchKeyword = ref('')
const activeStep = ref(0)

const navItems = ['首页', '流程百科', '成功案例', '医院评价', '关于我们']

const steps = [
  { title: '前期检查', description: '进行激素水平、卵巢功能、精液质量及遗传风险评估，医生据此制定个体化周期计划。' },
  { title: '促排卵', description: '通过药物刺激多个卵泡发育，并在超声与激素监测下动态调整剂量，平衡获卵数与安全性。' },
  { title: '取卵取精', description: '在麻醉或镇静下完成取卵，男性同步取精并进行精子优化处理，提升受精效率。' },
  { title: '胚胎培养', description: '实验室进行体外受精并连续培养，必要时进行囊胚培养与胚胎遗传学筛查。' },
  { title: '胚胎移植', description: '根据子宫内膜条件选择鲜胚或冻胚移植，过程微创、时间短，术后注意休息与药物支持。' },
  { title: '验孕', description: '移植后约 10-14 天检测 β-hCG，并在后续复查中确认妊娠进展与胚胎发育情况。' },
]

const infoCards = [
  { title: '试管费用如何构成？', desc: '从检查、促排药物、实验室操作到移植与保胎，拆解费用区间与影响因素。', icon: BookOpen },
  { title: '提高成功率的关键因素', desc: '结合年龄、胚胎质量、内膜状态与生活习惯，建立科学决策框架。', icon: HeartPulse },
  { title: '促排期间饮食与作息', desc: '如何安排蛋白质摄入、补水节奏和运动强度，减轻不适并稳定激素环境。', icon: Activity },
  { title: '胚胎实验室技术解读', desc: '一文了解 ICSI、囊胚培养、PGT 等核心技术的适应症与局限。', icon: Microscope },
  { title: '医院与医生如何选择', desc: '关注生殖中心资质、周期量、实验室能力与患者口碑，避免盲目跟风。', icon: ShieldCheck },
  { title: '第一次咨询要准备什么？', desc: '病历、检查报告、问题清单与沟通重点，一次问清楚提升决策效率。', icon: BadgeHelp },
]

const faqs = [
  { title: '做一次试管婴儿大概需要多少钱？', answer: '通常由检查、药物、实验室操作和移植组成，整体会因城市、医院级别和个体方案差异而波动，建议先完成评估后获取报价。' },
  { title: '试管婴儿成功率是多少？', answer: '成功率与年龄、卵巢储备、精子质量、胚胎质量及子宫环境密切相关。建议关注“单次移植临床妊娠率”及累计活产率。' },
  { title: '全过程需要多久？', answer: '一个完整周期通常约 1-2 个月，若采用冻胚策略或需先调理内膜，整体周期会延长。' },
]

onMounted(() => {
  const revealItems = document.querySelectorAll<HTMLElement>('.reveal-item')
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('animate-in')
          observer.unobserve(entry.target)
        }
      })
    },
    { threshold: 0.2 },
  )

  revealItems.forEach((item) => observer.observe(item))
})
</script>

<style scoped>
:global(html) {
  scroll-behavior: smooth;
}

.reveal-item {
  transform: translateY(18px);
  transition: all 0.6s ease;
}

.reveal-item.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.fade-up-enter-active,
.fade-up-leave-active {
  transition: all 0.35s ease;
}

.fade-up-enter-from,
.fade-up-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
