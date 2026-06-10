<script setup lang="ts">
import {
  MapPinIcon,
  PhoneIcon,
  WrenchScrewdriverIcon,
} from '@heroicons/vue/24/solid'
import { faGoogle, faWhatsapp } from '@fortawesome/free-brands-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { computed, onMounted, onUnmounted, ref, type Component } from 'vue'
import logoUrl from './assets/profile.png'

type LinkItem = {
  label: string
  description: string
  href: string
  analyticsEvent: string
  heroIcon?: Component
  brandIcon?: typeof faGoogle
}

declare global {
  interface Window {
    gtag?: (
      command: 'event',
      eventName: string,
      parameters?: Record<string, string | number | boolean>,
    ) => void
  }
}

const companyPhone = '+5543996301677'
const whatsappNumber = '5543996301677'
const mapsUrl = 'https://maps.google.com/'
const googleReviewUrl = 'https://g.page/r/CQPwDk4jgF5JEAI/review'
const mapEmbedUrl =
  'https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2179.216397259641!2d-51.15049383407447!3d-23.27979442107828!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x94eb45009ef677fb%3A0x495e80234e0ef003!2sM%20Santos%20-%20Mec%C3%A2nica%20Automotiva!5e0!3m2!1spt-BR!2sbr!4v1780935881888!5m2!1spt-BR!2sbr'

const formatPhone = (phone: string) => {
  const digits = phone.replace(/\D/g, '')
  const localNumber = digits.startsWith('55') ? digits.slice(2) : digits

  if (localNumber.length === 11) {
    return localNumber.replace(/(\d{2})(\d{5})(\d{4})/, '($1) $2-$3')
  }

  if (localNumber.length === 10) {
    return localNumber.replace(/(\d{2})(\d{4})(\d{4})/, '($1) $2-$3')
  }

  return phone
}

const displayCompanyPhone = formatPhone(companyPhone)

const profile = {
  name: 'MSantos',
  handle: '@m.santos.br',
  headline: 'Mecanica automotiva especializada em carros nacionais e importados. Qualidade e confianca para cuidar do seu veiculo.',
  location: 'Mecânica Automotiva',
}

var WhatsappUrl = new URL(`https://wa.me/${whatsappNumber}`)
WhatsappUrl.searchParams.set('text', 'Olá, vim do instagram e gostaria de agendar um serviço. Meu carro é um [marca/modelo] e estou enfrentando [descreva o problema]. Você poderia me ajudar?')

const bookingLink = {
  label: 'Mandar WhatsApp',
  description: 'Agende, tire duvidas e envie dados do veiculo',
  href: WhatsappUrl.toString(),
  analyticsEvent: 'click_whatsapp_agendamento',
}

const links: LinkItem[] = [
  {
    label: 'Ligar agora',
    description: displayCompanyPhone,
    href: `tel:${companyPhone}`,
    analyticsEvent: 'click_ligar_agora',
    heroIcon: PhoneIcon,
  },
  {
    label: 'Nos avalie no Google!',
    description: 'Conte como foi seu atendimento',
    href: googleReviewUrl,
    analyticsEvent: 'click_avaliar_google',
    brandIcon: faGoogle,
  },
]

const attentionTargets = ['booking', 'route', ...links.map((_, index) => `link-${index}`)]
const activeAttentionTarget = ref<string | null>(null)
let attentionTimer: number | undefined

const pickAttentionTarget = () => {
  const availableTargets = attentionTargets.filter((target) => target !== activeAttentionTarget.value)
  const nextIndex = Math.floor(Math.random() * availableTargets.length)
  activeAttentionTarget.value = availableTargets[nextIndex] ?? attentionTargets[0]
}

const trackButtonClick = (eventName: string, label: string) => {
  window.gtag?.('event', eventName, {
    event_category: 'linktree_button',
    event_label: label,
  })
}

const services = [
  'Troca de oleo',
  'Freios',
  'Suspensao',
  'Scanner',
  'Eletrica',
  'Revisao completa',
]

const openingHour = 8
const closingHour = 18
const closingSoonHour = 17

const businessStatus = computed(() => {
  const now = new Date()
  const day = now.getDay()
  const hour = now.getHours()
  const isWeekend = day === 0 || day === 6
  const isBusinessDay = day >= 1 && day <= 5
  const isOpen = isBusinessDay && hour >= openingHour && hour < closingHour

  if (isWeekend) {
    return {
      label: 'Estamos descansando no fim de semana. Deixe sua mensagem no WhatsApp e retornamos no proximo dia util.',
      tone: 'closed',
    }
  }

  if (isOpen && hour >= closingSoonHour) {
    return {
      label: 'Ainda estamos por aqui, mas encerramos as 18h. Chame no WhatsApp para garantir seu atendimento.',
      tone: 'soon',
    }
  }

  if (isOpen) {
    return {
      label: 'Estamos abertos agora. Pode chamar no WhatsApp ou ligar para falar com a oficina.',
      tone: 'open',
    }
  }

  if (day === 5 && hour >= closingHour) {
    return {
      label: 'Ja encerramos por hoje. Envie sua mensagem no WhatsApp e retornamos na segunda a partir das 8h.',
      tone: 'closed',
    }
  }

  if (hour < openingHour) {
    return {
      label: 'Ainda nao abrimos, mas ja pode deixar sua mensagem no WhatsApp. Comecamos os atendimentos as 8h.',
      tone: 'closed',
    }
  }

  return {
    label: 'Ja encerramos os atendimentos de hoje. Mande uma mensagem no WhatsApp e a gente retorna no proximo dia util.',
    tone: 'closed',
  }
})

onMounted(() => {
  pickAttentionTarget()
  attentionTimer = window.setInterval(pickAttentionTarget, 4200)
})

onUnmounted(() => {
  window.clearInterval(attentionTimer)
})
</script>

<template>
  <main class="garage-shell min-h-screen px-4 py-5 text-white sm:px-6">
    <section class="mx-auto flex min-h-[calc(100vh-2.5rem)] w-full max-w-[520px] items-center">
      <div class="linktree-card w-full rounded-lg border border-white/10 bg-[#141414]/95 p-4 shadow-2xl sm:p-5">
        <header class="animate-rise flex items-start gap-4">
          <div
            class="logo-mark grid h-[6rem] w-[6rem] shrink-0 place-items-center overflow-hidden rounded-lg border border-[#ff7200]/50 bg-white"
          >
            <img class="h-full w-full object-contain" :src="logoUrl" alt="Logo MSantos Auto Service" />
          </div>

          <div class="min-w-0 flex-1">
            <h1 class="text-3xl font-black leading-tight sm:text-6xl">
              {{ profile.name }}
            </h1>
            <p class="mt-1 text-lg font-bold uppercase tracking-[0.2em] text-[#ff7200]">
              {{ profile.location }}
            </p>
          </div>
        </header>

        <p class="animate-rise mt-4 text-sm leading-6 text-zinc-300 sm:text-base">
          {{ profile.headline }}
        </p>

        <div
          class="animate-rise noise-surface mt-4 flex items-center gap-2 rounded border px-3 py-2"
          :class="{
            'border-emerald-400/30 bg-emerald-400/10': businessStatus.tone === 'open',
            'border-[#ff7200]/30 bg-[#ff7200]/10': businessStatus.tone === 'soon',
            'border-white/10 bg-white/[0.04]': businessStatus.tone === 'closed',
          }"
        >
          <span
            class="status-light shrink-0"
            :class="{
              'status-light--open': businessStatus.tone === 'open',
              'status-light--soon': businessStatus.tone === 'soon',
              'status-light--closed': businessStatus.tone === 'closed',
            }"
            aria-hidden="true"
          ></span>
          <span
            class="text-sm font-bold"
            :class="{
              'text-emerald-300': businessStatus.tone === 'open',
              'text-[#ff7200]': businessStatus.tone === 'soon',
              'text-zinc-300': businessStatus.tone === 'closed',
            }"
          >
            {{ businessStatus.label }}
          </span>
        </div>

        <a
          :href="bookingLink.href"
          class="cta-pulse noise-surface group mt-4 flex min-h-24 items-center justify-between rounded-lg border border-[#ff7200] bg-[#ff7200] p-4 text-[#151515] shadow-xl shadow-black/30 transition hover:-translate-y-0.5 focus:outline-none focus:ring-2 focus:ring-[#ff7200] focus:ring-offset-2 focus:ring-offset-[#141414]"
          :class="{ 'attention-wiggle': activeAttentionTarget === 'booking' }"
          target="_blank"
          rel="noreferrer"
          @click="trackButtonClick(bookingLink.analyticsEvent, bookingLink.label)"
        >
          <span>
            <span class="block text-xl font-black">{{ bookingLink.label }}</span>
            <span class="mt-1 block text-sm font-semibold text-[#3a2a00]">
              {{ bookingLink.description }}
            </span>
          </span>
          <span
            class="ml-4 grid h-12 w-12 shrink-0 place-items-center rounded bg-[#151515] text-sm font-black text-[#ff7200] transition group-hover:translate-x-1"
            aria-hidden="true"
          >
            <FontAwesomeIcon :icon="faWhatsapp" class="h-6 w-6" />
          </span>
        </a>

        <nav class="animate-rise mt-3 grid gap-2" aria-label="Links principais">
          <a
            v-for="(link, index) in links"
            :key="link.label"
            :href="link.href"
            class="link-action noise-surface flex items-center gap-3 rounded-lg border border-white/10 bg-[#202020] p-3 transition hover:border-[#ff7200]/60 hover:bg-[#262626] focus:outline-none focus:ring-2 focus:ring-[#ff7200]"
            :class="{ 'attention-wiggle': activeAttentionTarget === `link-${index}` }"
            target="_blank"
            rel="noreferrer"
            @click="trackButtonClick(link.analyticsEvent, link.label)"
          >
            <span
              class="grid h-10 w-10 shrink-0 place-items-center rounded border border-[#ff7200]/30 bg-[#111] text-xs font-black text-[#ff7200]"
              aria-hidden="true"
            >
              <FontAwesomeIcon v-if="link.brandIcon" :icon="link.brandIcon" class="h-5 w-5" />
              <component :is="link.heroIcon" v-else class="h-5 w-5" />
            </span>
            <span class="min-w-0">
              <span class="block truncate text-sm font-black">{{ link.label }}</span>
              <span class="block truncate text-xs text-zinc-400">{{ link.description }}</span>
            </span>
          </a>
        </nav>

        <section class="animate-rise noise-surface mt-4 overflow-hidden rounded-lg border border-white/10 bg-[#202020]">
          <div class="flex items-center justify-between gap-3 p-3">
            <div>
              <p class="text-xs font-bold uppercase tracking-[0.2em] text-[#ff7200]">
                Como chegar
              </p>
              <p class="mt-1 text-sm text-zinc-400">Veja a localizacao da oficina.</p>
            </div>
            <a
              :href="mapsUrl"
              class="shrink-0 rounded bg-[#ff7200] px-3 py-2 text-xs font-black text-[#151515] transition hover:bg-[#ffd15c] focus:outline-none focus:ring-2 focus:ring-[#ff7200]"
              :class="{ 'attention-wiggle': activeAttentionTarget === 'route' }"
              target="_blank"
              rel="noreferrer"
              @click="trackButtonClick('click_como_chegar', 'Abrir rota')"
            >
              <span class="inline-flex items-center gap-1.5">
                <MapPinIcon class="h-4 w-4" aria-hidden="true" />
                Abrir rota
              </span>
            </a>
          </div>
          <iframe
            class="h-40 w-full border-0"
            :src="mapEmbedUrl"
            loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"
            title="Mapa com localizacao da oficina"
          ></iframe>
        </section>

        <section class="animate-rise mt-4">
          <p class="inline-flex items-center gap-2 text-xs font-bold uppercase tracking-[0.2em] text-zinc-500">
            <WrenchScrewdriverIcon class="h-4 w-4 text-[#ff7200]" aria-hidden="true" />
            Servicos
          </p>
          <div class="mt-2 flex flex-wrap gap-2">
            <span
              v-for="service in services"
              :key="service"
              class="noise-surface rounded-full border border-white/10 bg-white/[0.04] px-3 py-1.5 text-xs font-bold text-zinc-300"
            >
              {{ service }}
            </span>
          </div>
        </section>
      </div>
    </section>
  </main>
</template>
