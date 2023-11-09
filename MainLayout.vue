<template>
	<v-app id="root">

		<v-app-bar
			app
			color="primary"
			density="compact"
		>
			<template v-slot:prepend>
				<v-app-bar-nav-icon
					class="hidden-md-and-up"
					@click.stop="toggleDrawer"
				>
				</v-app-bar-nav-icon>
			</template>

			<v-app-bar-title class="headline text-uppercase">
				<router-link
					to="/"
					class="toolbar-title"
				>
					{{ $t('titles.application') }}
				</router-link>
			</v-app-bar-title>

			<v-btn
				v-if="$vuetify.display.mdAndUp && features.Rockets"
				to="/rockets"
			>
				{{ $t('menu.content.rockets.title') }}
			</v-btn>

			<MainMenuToolbar
				:displaySignIn="displaySignIn"
				:features="features"
				:info="info"
				:isLoggedIn="isLoggedIn"
				:tools="tools"
			/>

			<template v-slot:append>
				<SecondaryMenu
					:clickAbout="clickAbout"
					:clickOpenSource="clickOpenSource"
					:clickPrivcy="clickPrivcy"
					:clickSignOut="clickSignOut"
					:clickSupport="clickSupport"
					:displaySignIn="displaySignIn"
					:features="features"
					:info="info"
					:isLoggedIn="isLoggedIn"
					:tools="tools"
					@clickSignIn="clickSignIn"
					@clickSignOut="clickSignOut"
				 />
			</template>
		</v-app-bar>

		<v-navigation-drawer
			v-model="displayDrawer"
			temporary
		>
			<MainMenuDrawer
				:displaySignIn="displaySignIn"
				:features="features"
				:info="info"
				:isLoggedIn="isLoggedIn"
				:tools="tools"
			/>
		</v-navigation-drawer>

		<v-main id="top">
			<span class="bg" />
			<v-container
				fluid
				pt-2
			>
			<!-- [[ online = {{  online  }}]]
			[[ isLoggedIn = {{  isLoggedIn  }}]] -->
				<router-view />
			</v-container>
		</v-main>

		<VtConfirmationDialog
			:signal="dialogSignOut.signal"
			@cancel="dialogSignOut.cancel()"
			@ok="dialogSignOutOk"
		/>
		<!-- <VtDisplayDialog
			:signal="dialogDisplayMarkupSignal.signal"
			@cancel="dialogDisplayMarkupCancel"
			@ok="dialogDisplayMarkupOk"
		>
			!--eslint-disable vue/no-v-html --
			<div
				class="markdown-body"
				style="vertical-align: top;"
				v-html="displayMarkupValue"
			/>
			!--eslint-enable--
		</VtDisplayDialog> -->

		<v-footer
			v-if="displayFooter"
			app
		>
			<VtLayoutFooter />
		</v-footer>

		<VtLoadingOverlay
			:signal="isAuthCompleted"
		/>

		<VtCookieComply
			v-if="features.CookieComply"
			:preferences="preferences"
		/>
	</v-app>
</template>

<script>
import { computed } from 'vue';

import LibraryClientUtility from '@thzero/library_client/utility/index';

import VtCookieComply from '@thzero/library_client_vue3_vuetify3/components/VtCookieComply';
import VtConfirmationDialog from '@thzero/library_client_vue3_vuetify3/components/VtConfirmationDialog';
// import VtDisplayDialog from '@thzero/library_client_vue3_vuetify3/components/VtDisplayDialog';
import VtLayoutFooter from '@thzero/library_client_vue3_vuetify3/components/VtLayoutFooter';
import VtLoadingOverlay from '@thzero/library_client_vue3_vuetify3/components/VtLoadingOverlay';

import MainMenuDrawer from '@/components.app/main/mainMenuDrawer';
import MainMenuToolbar from '@/components.app/main/mainMenuToolbar';
import SecondaryMenu from '@/components.app/main/secondaryMenu';

import { useAppMainLayout } from '@/layouts/appMainLayout';

export default {
	name: 'MainLayout',
	components: {
		MainMenuDrawer,
		MainMenuToolbar,
		SecondaryMenu,
		VtConfirmationDialog,
		VtCookieComply,
		// VtDisplayDialog,
		VtLayoutFooter,
		VtLoadingOverlay
	},
	setup(props, context) {
		const {
			correlationId,
			error,
			hasFailed,
			hasSucceeded,
			initialize,
			logger,
			noBreakingSpaces,
			notImplementedError,
			success,
			features,
			closeOnContentClick,
			clickAbout,
			clickOpenSource,
			clickPrivcy,
			clickSignIn,
			clickSignOut,
			clickSupport,
			dialogSignOut,
			dialogSignOutOk,
			displayDrawer,
			displayFooter,
			isAuthCompleted,
			isLoggedIn,
			serviceAuth,
			serviceStore,
			toggleDrawer,
			contentLink,
			contentTitle,
			dialogDisplayMarkupCancel,
			dialogDisplayMarkupOk,
			dialogDisplayMarkupSignal,
			displayMarkupValue,
			dialogNewCharacter,
			info,
			displaySignIn,
			// links,
			markup,
			preferences,
			serviceMarkup,
			tools
		} = useAppMainLayout(props, context);

		const online = computed(() => {
			return LibraryClientUtility.$store.getters.getOnline();
		});

		return {
			correlationId,
			error,
			hasFailed,
			hasSucceeded,
			initialize,
			logger,
			noBreakingSpaces,
			notImplementedError,
			success,
			features,
			closeOnContentClick,
			clickAbout,
			clickOpenSource,
			clickPrivcy,
			clickSignIn,
			clickSignOut,
			clickSupport,
			dialogSignOut,
			dialogSignOutOk,
			displayDrawer,
			displayFooter,
			isAuthCompleted,
			isLoggedIn,
			serviceAuth,
			serviceStore,
			toggleDrawer,
			contentLink,
			contentTitle,
			dialogDisplayMarkupCancel,
			dialogDisplayMarkupOk,
			dialogDisplayMarkupSignal,
			displayMarkupValue,
			dialogNewCharacter,
			info,
			displaySignIn,
			// links,
			markup,
			preferences,
			serviceMarkup,
			tools,
			online
		};
	},
	// data: () => ({
	// 	closeOnContentClick: true,
	// 	// drawer: false,
	// 	dialogDisplayMarkupSignal: new DialogSupport(),
	// 	displayMarkupValue: null,
	// 	dialogNewCharacter: new DialogSupport()
	// }),
	// created() {
	// 	this._serviceMarkup = LibraryClientUtility.$injector.getService(LibraryClientConstants.InjectorKeys.SERVICE_MARKUP_PARSER);

	// 	LibraryClientUtility.$EventBus.on('display-markup', (value) => {
	// 		this.markup(this.correlationId(), value);
	// 		this.dialogDisplayMarkupSignal.open();
	// 	});
	// },
	// methods: {
	// 	async dialogDisplayMarkupCancel() {
	// 		this.dialogDisplayMarkupSignal.cancel();
	// 	},
	// 	async dialogDisplayMarkupOk() {
	// 		this.dialogDisplayMarkupSignal.ok();
	// 	},
	// 	markup(correlationId, value) {
	// 		this.displayMarkupValue = value ? this._serviceMarkup.trimResults(correlationId, this._serviceMarkup.render(correlationId, value)) : null;
	// 	}
	// }
};
</script>

<style scoped>
.toolbar-title {
	color: white;
	text-decoration: none;
}
</style>
