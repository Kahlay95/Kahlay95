using UnityEngine;

public class ItemPickUp : MonoBehaviour
{
    public Transform itemHolder; // Eşyayı tutacak olan karakterin elinde yer
    private GameObject pickedItem = null;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.E))
        {
            if (pickedItem == null)
            {
                TryPickUpItem();
            }
            else
            {
                DropItem();
            }
        }
    }

    void TryPickUpItem()
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        RaycastHit hit;
        if (Physics.Raycast(ray, out hit, 2f))
        {
            if (hit.collider.CompareTag("Item"))
            {
                pickedItem = hit.collider.gameObject;
                pickedItem.transform.SetParent(itemHolder);
                pickedItem.transform.localPosition = Vector3.zero;
                pickedItem.GetComponent<Rigidbody>().isKinematic = true;
            }
        }
    }

    void DropItem()
    {
        pickedItem.transform.SetParent(null);
        pickedItem.GetComponent<Rigidbody>().isKinematic = false;
        pickedItem = null;
    }
}
