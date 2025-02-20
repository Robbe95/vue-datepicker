<template>
    <div>
        <template v-if="$slots['month-year']">
            <slot
                name="month-year"
                v-bind="{
                    years: groupedYears,
                    selectYear,
                }"
            />
        </template>
        <template v-else>
            <selection-overlay
                :items="groupedYears"
                :is-last="autoApply && !defaultedConfig.keepActionRow"
                :height="defaultedConfig.modeHeight"
                :config="config"
                :no-overlay-focus="noOverlayFocus"
                :focus-value="focusYear"
                type="year"
                use-relative
                @selected="selectYear"
                @hover-value="setHoverValue"
            >
                <template v-if="$slots['year-overlay-value']" #item="{ item }">
                    <slot name="year-overlay-value" :text="item.text" :value="item.value" />
                </template>
            </selection-overlay>
        </template>
    </div>
</template>

<script lang="ts" setup>
    import SelectionOverlay from '@/components/Common/SelectionOverlay.vue';

    import { PickerBaseProps } from '@/props';
    import { useYearPicker } from '@/components/YearPicker/year-picker';
    import { useDefaults } from '@/composables';

    const emit = defineEmits(['update:internal-model-value', 'reset-flow', 'range-start', 'range-end', 'auto-apply']);
    const props = defineProps({
        ...PickerBaseProps,
    });

    defineOptions({
        compatConfig: {
            MODE: 3,
        },
    });

    const { groupedYears, modelValue, focusYear, selectYear, setHoverValue } = useYearPicker(props, emit);
    const { defaultedConfig } = useDefaults(props);

    const getSidebarProps = () => {
        return {
            modelValue,
            selectYear,
        };
    };

    defineExpose({ getSidebarProps });
</script>
