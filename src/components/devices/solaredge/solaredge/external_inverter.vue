<template>
	<div class="device-solaredge-external-inverter">
		<openwb-base-heading>
			Einstellungen für SolarEdge externen Wechselrichter
			<span class="small">(Modul: {{ $options.name }})</span>
		</openwb-base-heading>
		<openwb-base-alert subtype="info">
			Diese Komponente nur konfigurieren, wenn ein weiteres Solaredge
			SmartMeter verbaut ist, welches z.B. die Leistung einer vorhandenen
			Bestands-PV-Anlage erfasst. Dieses zusätzliche SmartMeter muss dann
			als "Zähler 2" / "Position 2" im Wechselrichter-Konfigurationsmenü
			konfiguriert sein.
		</openwb-base-alert>
		<openwb-base-number-input
			title="SolarEdge-Geräte-ID"
			required
			:model-value="configuration.modbus_id"
			min="1"
			max="255"
			@update:model-value="
				updateConfiguration($event, 'configuration.modbus_id')
			"
		/>
		<openwb-base-number-input
			title="SolarEdge-Meter-ID"
			:model-value="configuration.meter_id"
			min="1"
			max="255"
			@update:model-value="
				updateConfiguration($event, 'configuration.meter_id')
			"
		/>
		<openwb-base-select-input
			title="Leistung invertieren"
			notSelected="Bitte auswählen"
			:options="[
				{ value: 1, text: 'nein' },
				{ value: -1, text: 'ja' },
			]"
			:model-value="configuration.factor"
			@update:model-value="
				updateConfiguration($event, 'configuration.factor')
			"
		>
			<template #help>
				Einige externe SolarEdge Wechselrichter liefern die Leistung mit
				falschem Vorzeichen. In diesem Fall ist die Leistung zu
				invertieren.
			</template>
		</openwb-base-select-input>
	</div>
</template>

<script>
export default {
	name: "DeviceSolarEdgeExternalInverter",
	emits: ["update:configuration"],
	props: {
		configuration: { type: Object, required: true },
		deviceId: { default: undefined },
		componentId: { required: true },
	},
	methods: {
		updateConfiguration(event, path = undefined) {
			this.$emit("update:configuration", { value: event, object: path });
		},
	},
};
</script>
