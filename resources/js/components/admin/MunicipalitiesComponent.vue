<template>
	<div class="text-center">
		<a class="btn-simplex btn-simplex-md btn-simplex-primary" href=""
		   title="Registros de municipios" data-toggle="tooltip"
		   @click="addRecord('add_municipality', 'municipalities', $event)">
           <i class="fas fa-eye fa-3x"></i>
		   <span>Municipios</span>
		</a>
		<div class="modal fade text-left" tabindex="-1" role="dialog" id="add_municipality">
			<div class="modal-dialog modal-lg" role="document">
				<div class="modal-content">
					<div class="modal-header">
						<h6>
							<i class="fas fa-eye fa-3x"></i>
							Municipio
						</h6>
					</div>
					<div class="modal-body">
						<div class="alert alert-danger" v-if="errors.length > 0">
							<ul>
								<li v-for="error in errors">{{ error }}</li>
							</ul>
						</div>
                        <div class="row">
                            <div class="col-md-6">
        						<div class="form-group is-required">
                                    <label>País:</label>
									<select2 :options="countries" @input="getEstates"
                                        v-model="record.country_id">
                                    </select2>
        							<input type="hidden" v-model="record.id">
        	                    </div>
                            </div>
                            <div class="col-md-6">
        						<div class="form-group is-required">
                                    <label>Estado:</label>
									<select2 :options="estates" v-model="record.estate_id"></select2>
        	                    </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-md-6">
        						<div class="form-group is-required">
        							<label for="name">Código:</label>
        							<input type="text" id="code" placeholder="Código"
        								   class="form-control input-sm" v-model="record.code" data-toggle="tooltip"
        								   title="Indique el código (requerido)">
        	                    </div>
                            </div>
                            <div class="col-md-6">
        						<div class="form-group is-required">
        							<label for="name">Nombre:</label>
        							<input type="text" id="name" placeholder="Nombre"
        								   class="form-control input-sm" v-model="record.name" data-toggle="tooltip"
        								   title="Indique el nombre (requerido)">
        	                    </div>
                            </div>
                        </div>
	                </div>
					<div class="modal-footer">
	                	<div class="form-group">
	                		<modal-form-buttons :saveRoute="'municipalities'"></modal-form-buttons>
	                	</div>
	                </div>
	                <div class="modal-body modal-table">
	                	<v-client-table :columns="columns" :data="records" :options="table_options">
	                		<div slot="id" slot-scope="props" class="text-center">
	                			<button @click="initUpdate(props.row.id, $event)"
		                				class="btn btn-warning btn-xs btn-icon btn-action"
		                				title="Modificar registro" data-toggle="tooltip" type="button">
		                			<i class="fas fa-edit"></i>
		                		</button>
		                		<button @click="deleteRecord(props.row.id, 'municipalities')"
										class="btn btn-danger btn-xs btn-icon btn-action"
										title="Eliminar registro" data-toggle="tooltip"
										type="button">
									<i class="fas fa-trash"></i>
								</button>
	                		</div>
	                	</v-client-table>
	                </div>
		        </div>
		    </div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				record: {
					id: '',
					name: '',
                    code: '',
                    country_id: '',
                    estate_id: '',
				},
				errors: [],
				records: [],
                countries: [],
                estates: [],
				columns: ['estate.country.name', 'estate.name', 'name', 'id'],
			}
		},
		methods: {
			/**
			 * Método que borra todos los datos del formulario
			 *
			 * @author William Páez <paez.william8@gmail.com>
			 */
			reset() {
				this.record = {
					id: '',
					name: '',
                    code: '',
                    country_id: '',
                    estate_id: '',
				};
			},
		},
		created() {
			this.table_options.headings = {
				'estate.country.name': 'País',
                'estate.name': 'Estado',
                'name': 'Nombre',
				'id': 'Acción',
			};
			this.table_options.sortable = ['estate.country.name', 'estate.name'];
			this.table_options.filterable = ['estate.country.name', 'estate.name'];
			this.table_options.columnsClasses = {
				'estate.country.name': 'col-md-4',
                'estate.name': 'col-md-4',
                'name': 'col-md-2',
				'id': 'col-md-2',
			};
			this.getCountries();
            this.getEstates();
		},
	};
</script>
