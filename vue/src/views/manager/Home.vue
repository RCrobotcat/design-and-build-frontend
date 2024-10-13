<template>
  <div>
    <div class="card" style="padding: 15px; font-weight: bold; font-size: 15px;">
      Hello, welcome to Face Recognition Management System!
    </div>

    <div style="display: flex; margin: 10px 0">
      <div style="width: 50%;" class="card">
        <div style="margin-bottom: 30px; font-size: 20px; font-weight: bold">Notifications</div>
        <div>
          <el-timeline reverse slot="reference">
            <el-timeline-item v-for="item in notices" :key="item.id" :timestamp="item.time">
              <el-popover
                  placement="right"
                  width="200"
                  trigger="hover"
                  :content="item.content">
                <span slot="reference">{{ item.title }}</span>
              </el-popover>
            </el-timeline-item>
          </el-timeline>
        </div>
      </div>
      <div style="width: 30%; margin-left: 20px;" class="card">
        <div style="margin-bottom: 30px; font-size: 20px; font-weight: bold">Key Team Members</div>
        <div v-for="item in members" :key="item.id" style="display: flex; justify-content: space-between; margin-bottom: 10px;">
          <span style="margin-top: 20px;">{{ item.name }}</span>
          <span style="margin-left: 20px; margin-top: 20px;">{{ item.major }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
      // user: JSON.parse(localStorage.getItem('xm-user') || '{}'),
      notices: [
        {
          id: 1,
          time: '2023-10-01 10:00',
          title: 'System Maintenance',
          content: 'The system will be down for maintenance from 10:00 PM to 12:00 AM.'
        },
        {
          id: 2,
          time: '2023-10-02 14:00',
          title: 'New Feature Release',
          content: 'We have released a new feature for better user experience.'
        },
        {
          id: 3,
          time: '2023-10-03 09:00',
          title: 'Security Update',
          content: 'A new security update has been applied to the system.'
        },
        {
          id: 4,
          time: '2023-10-04 11:00',
          title: 'Database Backup',
          content: 'A scheduled database backup will occur at 11:00 AM.'
        },
        {
          id: 5,
          time: '2023-10-05 15:00',
          title: 'Server Upgrade',
          content: 'The server will be upgraded to improve performance.'
        },
        {
          id: 6,
          time: '2023-10-06 08:00',
          title: 'User Training',
          content: 'A user training session will be held at 8:00 AM.'
        },
        {
          id: 7,
          time: '2023-10-07 13:00',
          title: 'Bug Fixes',
          content: 'Several bugs have been fixed in the latest update.'
        },
        {id: 8, time: '2023-10-08 16:00', title: 'Policy Update', content: 'The privacy policy has been updated.'},
        {
          id: 9,
          time: '2023-10-11 10:00',
          title: 'Feature Deprecation',
          content: 'An old feature will be deprecated in the next release.'
        }
      ],
      members: [
        {id: 1, name: 'Kaiyuan Peng', major: 'IST'},
        {id: 2, name: 'Yujie Liu', major: 'IST'},
        {id: 3, name: 'Tengwei Bi', major: 'EIE'},
        {id: 4, name: 'Jianfeng Lin', major: 'EIE'},
        {id: 5, name: 'Keyu Qian', major: 'IST'},
        {id: 6, name: 'Yingjie Huang', major: 'IST'},
        {id: 7, name: 'Tiantong Song', major: 'EIE'},
      ]
    }
  },
  created() {
    // this.$request.get('/notice/selectAll').then(res => {
    //   this.notices = res.data || []
    // })
  }
}
</script>
