using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class HurtBox : MonoBehaviour
{
    // It's not pretty, not having all the stats in one place, but it's fast coding.
    [SerializeField] private float _damage = 6f;
    private List<Health> _damagedTargets;
    [SerializeField] private LayerMask _targetLayer;
    [SerializeField] private float _existTime = 1f;

    private void OnEnable()
    {
        _damagedTargets = new List<Health>();
        StartCoroutine(WaitForDisable());
    }
    
    private void OnTriggerUpdate(Collider other)
    {
        if ((_targetLayer & (1 << other.gameObject.layer))  != 0)
        {
            if (other.TryGetComponent(out Health targetHealth))
            {
                if (!_damagedTargets.Contains(targetHealth))
                {
                    targetHealth.Damage(_damage);
                    _damagedTargets.Add(targetHealth);
                }
            }
        }
    }

    private IEnumerator WaitForDisable()
    {
        yield return new WaitForSeconds(_existTime);
        gameObject.SetActive(false);
    }
}
