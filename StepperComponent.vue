<template>
    <div class="q-pa-md">
        <q-stepper v-model="step" ref="stepper" color="primary" animated>
            <q-step v-for="item in dataStep" :key="item.name" :name="item.name" :title="item.title" :caption="item.caption"
                :icon="item.icon" :disable="item.disable" :done="step > item.name">
                <div class="step-description-container">
                    {{ item.description }}
                </div>
                <component v-if="item.component" :is="item.component" v-bind="item.props"
                    @closeModalUserStepper="emitSaveObj(item, $event)" />

            </q-step>

            <template v-slot:navigation>
                <q-stepper-navigation>
                    <q-btn v-if="showFinishButton" @click="nextStep($refs.stepper, dataStep[step])" color="primary"
                        :label="step === dataStep.length ? 'Finalizar' : 'Siguiente'" />
                    <q-btn v-if="step > 1" flat color="primary" @click="previusStep($refs.stepper, dataStep[step - 2])"
                        label="Atras" class="q-ml-sm" />
                </q-stepper-navigation>
            </template>
        </q-stepper>
    </div>
</template>
  
<script>
import { ref, onMounted } from 'vue'

export default {
    name: 'StepperComponent',
    props: {
        dataStep: {
            type: Array,
            default: () => []
        }

    },
    setup(props, { emit }) {
        const step = ref(1);
        const showFinishButton = ref(true);

        const previusStep = (value, data) => {
            // console.log("desde el previus: ", data)
            if (data.props.saveObjtEmit != true) {
                showFinishButton.value = true;
            } else {
                showFinishButton.value = false;
            }
            value.previous();
        }
        const nextStep = (value, data) => {
            // para controlar el btn de siguiente

            // console.log(data);
            if (props.dataStep.length == step.value) {
                // console.log("Finish");

            } else {
                value.next();
                if (props.dataStep.length == step.value && data.props.saveObjtEmit) {
                    showFinishButton.value = false;
                } else {
                    if (data.props.saveObjtEmit != true) {
                        showFinishButton.value = true;
                    } else {
                        showFinishButton.value = false;
                    }
                }

            }


        }
        const emitSaveObj = (item, val) => {
            // console.log("el props: ", item.props.saveObjtEmit)
            if ((item.props.saveObjtEmit != undefined && item.props.saveObjtEmit == true) && (step.value == props.dataStep.length)) {
                // console.log("el valor desde el paso a paso: ", val);

                emit('CloseModalEmit', val)

            } else {
                if ((item.props.saveObjtEmit != undefined && item.props.saveObjtEmit == true) || (item.props.skipObj)) {
                    step.value += 1;
                    if (props.dataStep[step.value - 1].props.saveObjtEmit != true) {
                        showFinishButton.value = true;
                    }
                }

            }

        }
        onMounted(() => {

            if (props.dataStep[step.value - 1].props.saveObjtEmit) {
                // console.log("Al inicio de todo: ", props.dataStep[step.value - 1]);
                showFinishButton.value = false;
            }
        });

        return {
            step,
            nextStep,
            emitSaveObj,
            showFinishButton,
            previusStep
        }
    }
}
</script>
<style scoped>
.step-description-container {
    text-align: center;
}
</style>