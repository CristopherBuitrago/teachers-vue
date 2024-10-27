<template>
  <section>
    <h3>Add Teacher</h3>
    <div>
      <label>Name</label>
      <input type="text" v-model="teacher.teacherName" />
    </div>
    <div>
      <label>Last Name</label>
      <input type="text" v-model="teacher.teacherLastName" />
    </div>
    <div>
      <label>DNI / NIF</label>
      <input type="text" v-model="teacher.teacherDni" />
    </div>
    <div>
      <label>Subjects</label>
      <input type="text" v-model="subject" />
      <button @click="addSubject()">Add subject</button>
    </div>
    <div v-show="teacher.subjects && teacher.subjects.length > 0">
      <h4>Added subjects</h4>
      <ul>
        <li v-for="(subj, index) in teacher.subjects" :key="index">
          {{ subj }}
        </li>
      </ul>
    </div>
    <input type="checkbox" v-model="teacher.hasDocs" /> Docs sent
    <br />
    <button @click="addTeacher()">Add teacher</button>
  </section>

  <section>
    <h3>Teacher list</h3>
    <table>
      <tr>
        <th>Name</th>
        <th>Last Name</th>
        <th>DNI / NIF</th>
        <th>Subjects</th>
        <th>Docs sent</th>
      </tr>
      <tr v-for="tcher in teachers" :key="tcher.teacherDni">
        <td>{{ tcher.teacherName }}</td>
        <td>{{ tcher.teacherLastName }}</td>
        <td>{{ tcher.teacherDni }}</td>
        <td>
          <ul>
            <li v-for="(subject, index) in tcher.subjects" :key="index">{{ subject }}</li>
          </ul>
        </td>
        <td>{{ tcher.hasDocs ? 'Sent' : 'Not sent' }}</td>
      </tr>
    </table>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref, Ref } from 'vue';

interface ITeacher {
  teacherName: string;
  teacherLastName: string;
  teacherDni: string;
  subjects: Array<string>;
  hasDocs: boolean;
}

export default defineComponent({
  name: 'TeacherForm',
  setup() {
    // teachers info
    const teacher: Ref<ITeacher> = ref({
      teacherName: '',
      teacherLastName: '',
      teacherDni: '',
      subjects: [],
      hasDocs: true,
    });

    // teachers list
    const teachers: Ref<Array<ITeacher>> = ref([]);

    // subject added
    const subject: Ref<string> = ref('');

    // function to add a subject
    const addSubject = () => {
      if (subject.value.trim()) {
        teacher.value.subjects.push(subject.value.trim());
        subject.value = '';
      }
    };

    // function to add a teacher
    const addTeacher = () => {
      // Clone the teacher object to avoid reference issues
      teachers.value.push({ ...teacher.value });

      // Clear the teacher object
      teacher.value.teacherName = '';
      teacher.value.teacherLastName = '';
      teacher.value.teacherDni = '';
      teacher.value.subjects = [];
      teacher.value.hasDocs = false;
    };

    // return reactive data
    return {
      teacher,
      teachers,
      subject,
      addSubject,
      addTeacher,
    };
  },
});
</script>

<style scoped>
/* general styles */
section {
  margin: 20px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
}

/* form styles */
h3 {
  color: #333;
  font-size: 1.5em;
}

div {
  margin-bottom: 15px;
}

label {
  display: inline-block;
  width: 100px;
  font-weight: bold;
  color: #555;
}

input[type="text"],
button {
  padding: 8px;
  margin-left: 5px;
  border-radius: 4px;
  border: 1px solid #868686;
}

input[type="text"] {
  width: 200px;
}

button {
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

input[type="checkbox"] {
  margin-left: 5px;
}

/* subject list styles */
ul {
  padding-left: 20px;
}

li {
  color: #333;
}

/* table styles */
table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #111;
  margin-top: 20px;
}

th,
td {
  padding: 10px;
  border: 1px solid #ddd;
  text-align: left;
}

th {
  background-color: #f1f1f1;
  font-weight: bold;
}

td {
  background-color: #fff;
}

tr:nth-child(even) td {
  background-color: #f9f9f9;
}

/* hover */
button:hover {
  background-color: #0056b3;
}
</style>
