<script>
	import { animateIn } from '$lib/Helper.mjs'
	import archLogo from '$lib/images/logos/arch.svg'
	import nixLogo from '$lib/images/logos/nixos.svg'
	import bsdLogo from '$lib/images/logos/freebsd.svg'
	import suseLogo from '$lib/images/logos/opensuse.svg'
	import LinkOutIcon from '~icons/akar-icons/link-out'
	import InstallButton from './InstallButton.svelte'
	import Button from '$lib/components/Button.svelte'
	import Title from '$lib/components/Title.svelte'
	import amongus from '$lib/images/amongus/green.webp'
</script>

<section class="pb-6">
	<Title class="mb-8">
		<span slot="title">Install now</span><span slot="subtitle"> For your favourite distro </span>
	</Title>

	<div class="flex flex-col items-center gap-12 md:gap-6" use:animateIn={{ slide: 24, fade: 0 }}>
		<div
			class="links_ flex flex-col gap-12 px-4 md:gap-6 md:rounded-3xl md:bg-gradient-to-tr md:from-blue-500/40 md:to-transparent md:p-8 md:shadow-xl md:outline md:outline-1 md:outline-blue-500"
		>
			<InstallButton name="Arch" command="pacman -S hyprland" image={archLogo}
				><div slot="extra">
					AUR git version: <a
						class=" "
						target="_blank"
						href="https://aur.archlinux.org/packages/hyprland-git/">hyprland-git</a
					>
				</div></InstallButton
			>
			<InstallButton name="NixOS" command="programs.hyprland.enable" image={nixLogo}
				><div slot="extra">
					<a href="https://wiki.hyprland.org/Nix/" target="_blank"
						>See more details and git version</a
					>
				</div>
			</InstallButton>
			<InstallButton name="FreeBSD" command="pkg install hyprland" image={bsdLogo} />
			<InstallButton name="openSUSE" command="zypper in hyprland" image={suseLogo}
				><div slot="extra">or install “hyprland” via YaST2 Software.</div>

				<img
					class=" absolute inset-0 -z-10 translate-y-1 rotate-0 scale-90 opacity-0 transition-all duration-700 [transition-delay:2s] group-hover:-translate-x-3 group-hover:-translate-y-0 group-hover:-rotate-12 group-hover:scale-100 group-hover:opacity-90"
					src={amongus}
					slot="imageExtra"
					alt=""
					srcset=""
				/>
			</InstallButton>
		</div>

		<a
			target="_blank"
			href="https://wiki.hyprland.org/Getting-Started/Installation/#packages"
			class="mt-10"
		>
			<Button size="lg" type="fancyOutline">Other options</Button>
		</a>
	</div>
</section>

<style lang="postcss">
	.links_ {
		a {
			@apply font-medium hover:text-white hover:underline;
		}
	}
</style>
