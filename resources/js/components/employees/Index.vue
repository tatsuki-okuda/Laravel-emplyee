<template>
    <div>
            <!-- Page Heading -->
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">employees</h1>
    </div>
    <div class="row">
        <div class="card mx-auto">

            <div v-if="showMsg">
                <div class="alert alert-success">
                    {{ msg }}
                </div>
            </div>

            <div class="card-header">
                <div class="row">
                    <div class="col">
                        <form >
                            <div class="form-row align-items-center">
                                <div class="col">
                                    <input type="search" v-model.lazy="search" class="form-control mb-2" 
                                        placeholder="Jane Doe">
                                </div>
                                <div class="col">
                                    <button type="submit" class="btn btn-primary mb-2">Search</button>
                                </div>
                                <div class="col">
                                     <select 
                                        v-model="selectedDepartment"
                                        name="state" 
                                        class="form-control" 
                                        aria-label="Default select example">
                                        <option v-for="department in departments" :key="department.id" :value="department.id">
                                            {{ department.name }}
                                        </option>
                                    </select>
                                </div>
                            </div>
                        </form>
                    </div>
                    <div>
                        <router-link :to="{ name: 'EmployeesCreate' }" class="btn btn-primary mb-2">Create</router-link>
                    </div>
                </div>
            </div>
            
            <div class="card-body">
                <table class="table">
                    <thead>
                      <tr>
                        <th scope="col">#id</th>
                        <th scope="col">First Name</th>
                        <th scope="col">Last Name</th>
                        <th scope="col">Address</th>
                        <th scope="col">Department</th>
                        <th scope="col">Manage</th>
                      </tr>
                    </thead>
                    <tbody>
                        <tr v-for="employee in employees" :key="employee.id">
                            <th scope="row">{{ employee.id }}</th>
                            <td>{{ employee.first_name }}</td>
                            <td>{{ employee.last_name }}</td>
                            <td>{{ employee.address }}</td>
                            <td>{{ employee.department.name }}</td>
                            <td>
                                <router-link :to="{ name: 'EmployeesEdit', params: { id: employee.id } }" class="btn btn-success">Edit</router-link>
                                <button class="btn btn-danger" @click="deleteEmployee(employee.id)">Delete</button>
                            </td>
                        </tr>
                    </tbody>
                  </table>
            </div>
        </div>
    </div>
    </div>
</template>


<script>
export default {
    data(){
        return {
            employees: [],
            showMsg: false,
            msg: '',
            search: null,
            selectedDepartment: null,
            departments: []
        }
    },
    watch: {
        search(){
            this.getEmpoloyees()
        },
        selectedDepartment(){
            this.getEmpoloyees()
        }
    },
    created(){
        this.getEmpoloyees()
        this.getDepartments()
    },
    methods: {
        getEmpoloyees(){
            axios
                .get('/api/employees', {params: {
                    search: this.search,
                    department_id: this.selectedDepartment
                }})
                .then(res => {
                    this.employees = res.data.data
                })
                .catch(err => {
                    console.log(err)
                })
        },
        deleteEmployee(employeeId){
             axios
                .delete('/api/employees/'+ employeeId)
                .then(res => {
                    console.log(res)
                    this.showMsg = true
                    this.msg = res.data
                    this.getEmpoloyees()
                })
                .catch(err => {
                    console.log(err)
                })
        },
        getDepartments(){
          axios
            .get("/api/employees/departments")
            .then(res => {
                this.departments = res.data;
            })
            .catch(error => {
                console.log(console.error);
            });
      },
    }
}
</script>