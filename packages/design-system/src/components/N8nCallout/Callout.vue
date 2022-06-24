<template>
	<div :class="classes" role="alert">

		<div :class="$style['message-section']">
			<div :class="$style.icon">
				<n8n-icon
					:icon="getIcon"
					:size="theme === 'secondary' ? 'medium' : 'large'"
				/>
			</div>
			<slot />
			<div :class="$style['actions']">
				<slot name="actions" />
			</div>
		</div>

		<slot name="trailingContent" />
	</div>
</template>

<script lang="ts">
import Vue from 'vue';
import N8nIcon from '../N8nIcon';
import N8nLink from '../N8nLink';
import N8nText from '../N8nText';

const CALLOUT_DEFAULT_ICONS = {
	info: 'info-circle',
	success: 'check-circle',
	warning: 'exclamation-triangle',
	danger: 'times-circle',
};

export default Vue.extend({
	name: 'n8n-callout',
	components: {
		N8nIcon,
		N8nLink,
		N8nText
	},
	props: {
		theme: {
			type: String,
			required: true,
			validator: (value: string): boolean =>
				['info', 'success', 'secondary', 'warning', 'danger', 'custom'].includes(value),
		},
		icon: {
			type: String,
			default: 'info-circle'
		},
	},
	computed: {
		classes(): string[] {
			return [
				this.$style['callout'],
				this.$style[this.theme],
			];
		},
		getIcon(): string {
			if (Object.keys(CALLOUT_DEFAULT_ICONS).includes(this.theme)) {
				return CALLOUT_DEFAULT_ICONS[this.theme];
			}

			return this.icon;
		},
	}
});
</script>

<style lang="scss" module>
.callout {
	display: flex;
	justify-content: space-between;
	font-size: var(--font-size-2xs);
	padding: var(--spacing-xs);
	border: var(--border-width-base) var(--border-style-base);
	border-radius: var(--border-radius-base);
	align-items: center;
	line-height: var(--font-line-height-loose);
}

.message-section {
	display: flex;
}

.actions {
	color: green;
	padding-left: var(--spacing-4xs);
	text-decoration: underline;
}

.info, .custom {
	border-color: var(--color-foreground-base);
	background-color: var(--color-background-light);
	color: var(--color-info);
}

.warning {
	border-color: var(--color-warning-tint-1);
	background-color: var(--color-warning-tint-2);
	color: var(--color-warning);
}

.success {
	border-color: var(--color-success-tint-1);
	background-color: var(--color-success-tint-2);
	color: var(--color-success);
}

.danger {
	border-color: var(--color-danger-tint-1);
	background-color: var(--color-danger-tint-2);
	color: var(--color-danger);
}

.icon {
	margin-right: var(--spacing-xs);
}

.secondary {
	font-size: var(--font-size-2xs);
	font-weight: var(--font-weight-bold);
	color: var(--color-secondary);
	background-color: var(--color-secondary-tint-2);
	border-color: var(--color-secondary-tint-1);
}
</style>