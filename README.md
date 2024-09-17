# stepper-component-vue with quasar

### Step-by-step allows rendering of dynamic components in addition to static content

# data structure
```
    const steps = [
            {
                name: 1,
                title: "Crear Persona",
                icon: "person",
                description: "Si la persona esta creada puede continuar",
                disable: false,
                component: FormPersonComponent,
                props: {
                    user: userForm.user,
                    saveObjtEmit: false,
                    skipObj: true,
                },
            },
            {
                name: 2,
                title: "Create Usuario",
                icon: "account_box",
                disable: false,
                component: FormUserComponent,
                props: {
                    user: userForm.user,
                    saveObjtEmit: true,
                    rechargeFunction: () => toListUsers(),
                },
            },
        ];
```

# how to use
```
<div class="ModalStepper">
    <q-dialog v-model="fixed" persistent>
        <q-card style="width: 1207px; max-width: 90vw; height: 800px">
            <q-card-section>
                <div class="text-h6">
                    <div class="header-form">
                        <div></div>
                        <q-btn color="negative" label="X" v-close-popup>
                            <q-tooltip>Cerrar ventana</q-tooltip>
                        </q-btn>
                    </div>
                </div>
            </q-card-section>

            <q-separator />

            <q-card-section style="max-height: 88vh" class="scroll">
                <StepperComponent :dataStep="steps" @CloseModalEmit="closeModalStep" />
            </q-card-section>

            <q-separator />
        </q-card>
    </q-dialog>
</div>
```
# saveObjtEmit and skipObj
saveObjtEmit: If it is true, it means that it will save data or an action to move on to the next step.,
skipObj: If it is true, the next button appears and we move on to the next step. If this is false, it does not allow passage if an action is not executed in the child component,
# emits
emit("closeModalUserStepper", false);
With this emit from the child we can close the stepper window from the child component
